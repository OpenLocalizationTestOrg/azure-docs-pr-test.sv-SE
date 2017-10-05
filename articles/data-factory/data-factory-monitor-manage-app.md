---
title: "Övervaka och hantera data pipelines - Azure | Microsoft Docs"
description: "Lär dig hur du använder appen för hantering och övervakning för att övervaka och hantera Azure datafabriker och rörledningar."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: d5a2d1f3d85b8a2212326cfcfd0ba5d80356b769
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-monitoring-and-management-app"></a><span data-ttu-id="4f507-103">Övervaka och hantera Azure Data Factory pipelines med hjälp av övervakning och hantering av appen</span><span class="sxs-lookup"><span data-stu-id="4f507-103">Monitor and manage Azure Data Factory pipelines by using the Monitoring and Management app</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4f507-104">Med hjälp av Azure portal/Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4f507-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="4f507-105">Med hjälp av övervakning och Management-appen</span><span class="sxs-lookup"><span data-stu-id="4f507-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)
>
>

<span data-ttu-id="4f507-106">Den här artikeln beskriver hur du använder appen för hantering och övervakning för att övervaka, hantera och felsöka din Data Factory pipelines.</span><span class="sxs-lookup"><span data-stu-id="4f507-106">This article describes how to use the Monitoring and Management app to monitor, manage, and debug your Data Factory pipelines.</span></span> <span data-ttu-id="4f507-107">Den innehåller också information om hur du skapar aviseringar om du vill få information om fel.</span><span class="sxs-lookup"><span data-stu-id="4f507-107">It also provides information on how to create alerts to get notified on failures.</span></span> <span data-ttu-id="4f507-108">Du kan komma igång med hjälp av programmet genom att titta på nedanstående video:</span><span class="sxs-lookup"><span data-stu-id="4f507-108">You can get started with using the application by watching the following video:</span></span>

> [!NOTE]
> <span data-ttu-id="4f507-109">Användargränssnittet som visas i videon kanske inte stämmer exakt vad som visas i portalen.</span><span class="sxs-lookup"><span data-stu-id="4f507-109">The user interface shown in the video may not exactly match what you see in the portal.</span></span> <span data-ttu-id="4f507-110">Det är något äldre men begrepp förblir detsamma.</span><span class="sxs-lookup"><span data-stu-id="4f507-110">It's slightly older, but concepts remain the same.</span></span> 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-the-monitoring-and-management-app"></a><span data-ttu-id="4f507-111">Starta övervaknings- och Management-appen</span><span class="sxs-lookup"><span data-stu-id="4f507-111">Launch the Monitoring and Management app</span></span>
<span data-ttu-id="4f507-112">Om du vill starta appen Övervakare och hantering, klickar du på den **övervaka och hantera** panelen på den **Data Factory** bladet för din data factory.</span><span class="sxs-lookup"><span data-stu-id="4f507-112">To launch the Monitor and Management app, click the **Monitor & Manage** tile on the **Data Factory** blade for your data factory.</span></span>

![Övervakning av panelen på startsidan Data Factory](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

<span data-ttu-id="4f507-114">Du bör se övervakning och hantering av appen öppnas i ett separat fönster.</span><span class="sxs-lookup"><span data-stu-id="4f507-114">You should see the Monitoring and Management app open in a separate window.</span></span>  

![Övervaknings- och hanteringsapp](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> <span data-ttu-id="4f507-116">Om du ser att webbläsaren har ”auktorisera...”, avmarkera den **blockerar cookies från tredje part och platsdata** kryssrutan-- eller behålla den har markerats kan du skapa ett undantag för **login.microsoftonline.com**, och Försök att öppna appen igen.</span><span class="sxs-lookup"><span data-stu-id="4f507-116">If you see that the web browser is stuck at "Authorizing...", clear the **Block third-party cookies and site data** check box--or keep it selected, create an exception for **login.microsoftonline.com**, and then try to open the app again.</span></span>


<span data-ttu-id="4f507-117">I listan aktivitet Windows i den mellersta rutan finns en aktivitetsfönstret för varje körning av en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="4f507-117">In the Activity Windows list in the middle pane, you see an activity window for each run of an activity.</span></span> <span data-ttu-id="4f507-118">Om du har aktiviteten ska köras varje timme för fem timmar finns till exempel fem aktivitetsfönster som är associerade med fem datasektorer.</span><span class="sxs-lookup"><span data-stu-id="4f507-118">For example, if you have the activity scheduled to run hourly for five hours, you see five activity windows associated with five data slices.</span></span> <span data-ttu-id="4f507-119">Om du inte ser aktivitet windows i listan längst ned, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="4f507-119">If you don't see activity windows in the list at the bottom, do the following:</span></span>
 
- <span data-ttu-id="4f507-120">Uppdatering av **starttid** och **sluttiden** filter längst upp för att matcha start- och sluttider för din pipeline och klicka sedan på den **tillämpa** knappen.</span><span class="sxs-lookup"><span data-stu-id="4f507-120">Update the **start time** and **end time** filters at the top to match the start and end times of your pipeline, and then click the **Apply** button.</span></span>  
- <span data-ttu-id="4f507-121">Listan över Windows aktivitet uppdateras inte automatiskt.</span><span class="sxs-lookup"><span data-stu-id="4f507-121">The Activity Windows list is not automatically refreshed.</span></span> <span data-ttu-id="4f507-122">Klicka på den **uppdatera** i verktygsfältet i den **aktivitet Windows** lista.</span><span class="sxs-lookup"><span data-stu-id="4f507-122">Click the **Refresh** button on the toolbar in the **Activity Windows** list.</span></span>  

<span data-ttu-id="4f507-123">Om du inte har ett Data Factory-program för att testa dessa steg med, gör kursen: [kopiera data från Blob Storage till SQL-databas med hjälp av Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="4f507-123">If you don't have a Data Factory application to test these steps with, do the tutorial: [copy data from Blob Storage to SQL Database using Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="understand-the-monitoring-and-management-app"></a><span data-ttu-id="4f507-124">Förstå övervakning och Management-appen</span><span class="sxs-lookup"><span data-stu-id="4f507-124">Understand the Monitoring and Management app</span></span>
<span data-ttu-id="4f507-125">Det finns tre flikar till vänster: **Resursläsaren**, **övervakning vyer**, och **aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="4f507-125">There are three tabs on the left: **Resource Explorer**, **Monitoring Views**, and **Alerts**.</span></span> <span data-ttu-id="4f507-126">Den första fliken (**Resursläsaren**) väljs som standard.</span><span class="sxs-lookup"><span data-stu-id="4f507-126">The first tab (**Resource Explorer**) is selected by default.</span></span>

### <a name="resource-explorer"></a><span data-ttu-id="4f507-127">Resource Explorer</span><span class="sxs-lookup"><span data-stu-id="4f507-127">Resource Explorer</span></span>
<span data-ttu-id="4f507-128">Du ser följande:</span><span class="sxs-lookup"><span data-stu-id="4f507-128">You see the following:</span></span>

* <span data-ttu-id="4f507-129">Resursläsaren **trädvy** i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="4f507-129">The Resource Explorer **tree view** in the left pane.</span></span>
* <span data-ttu-id="4f507-130">Den **diagramvyn** längst upp i den mellersta rutan.</span><span class="sxs-lookup"><span data-stu-id="4f507-130">The **Diagram View** at the top in the middle pane.</span></span>
* <span data-ttu-id="4f507-131">Den **aktivitet Windows** listan längst ned i den mellersta rutan.</span><span class="sxs-lookup"><span data-stu-id="4f507-131">The **Activity Windows** list at the bottom in the middle pane.</span></span>
* <span data-ttu-id="4f507-132">Den **egenskaper**, **aktivitet fönstret Explorer**, och **skriptet** flikar i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="4f507-132">The **Properties**, **Activity Window Explorer**, and **Script** tabs in the right pane.</span></span>

<span data-ttu-id="4f507-133">I resursutforskaren visas alla resurser (pipelines, datauppsättningar, länkade tjänster) i datafabriken i en trädvy.</span><span class="sxs-lookup"><span data-stu-id="4f507-133">In Resource Explorer, you see all resources (pipelines, datasets, linked services) in the data factory in a tree view.</span></span> <span data-ttu-id="4f507-134">När du väljer ett objekt i Resursläsaren:</span><span class="sxs-lookup"><span data-stu-id="4f507-134">When you select an object in Resource Explorer:</span></span>

* <span data-ttu-id="4f507-135">Den associerade Data Factory-posten är markerad i diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="4f507-135">The associated Data Factory entity is highlighted in the Diagram View.</span></span>
* <span data-ttu-id="4f507-136">[Associerade aktiviteten windows](data-factory-scheduling-and-execution.md) är markerade i listan över aktiviteten Windows längst ned.</span><span class="sxs-lookup"><span data-stu-id="4f507-136">[Associated activity windows](data-factory-scheduling-and-execution.md) are highlighted in the Activity Windows list at the bottom.</span></span>  
* <span data-ttu-id="4f507-137">Egenskaper för det markerade objektet visas i fönstret Egenskaper i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="4f507-137">The properties of the selected object are shown in the Properties window in the right pane.</span></span>
* <span data-ttu-id="4f507-138">JSON-definitionen för det markerade objektet visas, om tillämpligt.</span><span class="sxs-lookup"><span data-stu-id="4f507-138">The JSON definition of the selected object is shown, if applicable.</span></span> <span data-ttu-id="4f507-139">Exempel: en länkad tjänst, ett dataset eller en pipeline.</span><span class="sxs-lookup"><span data-stu-id="4f507-139">For example: a linked service, a dataset, or a pipeline.</span></span>

![Resource Explorer](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

<span data-ttu-id="4f507-141">Finns det [schemaläggning och körning](data-factory-scheduling-and-execution.md) artikel detaljerad konceptuell information om aktiviteten windows.</span><span class="sxs-lookup"><span data-stu-id="4f507-141">See the [Scheduling and Execution](data-factory-scheduling-and-execution.md) article for detailed conceptual information about activity windows.</span></span>

### <a name="diagram-view"></a><span data-ttu-id="4f507-142">Diagramvy</span><span class="sxs-lookup"><span data-stu-id="4f507-142">Diagram View</span></span>
<span data-ttu-id="4f507-143">Diagramvy för en datafabrik ger en och samma plats att övervaka och hantera en datafabrik och dess tillgångar.</span><span class="sxs-lookup"><span data-stu-id="4f507-143">The Diagram View of a data factory provides a single pane of glass to monitor and manage a data factory and its assets.</span></span> <span data-ttu-id="4f507-144">När du väljer en Data Factory-entitet (dataset/pipeline) i diagramvyn:</span><span class="sxs-lookup"><span data-stu-id="4f507-144">When you select a Data Factory entity (dataset/pipeline) in the Diagram View:</span></span>

* <span data-ttu-id="4f507-145">Data factory-entiteten är valt i trädvyn.</span><span class="sxs-lookup"><span data-stu-id="4f507-145">The data factory entity is selected in the tree view.</span></span>
* <span data-ttu-id="4f507-146">Den associerade aktivitet windows markeras i listan över aktiviteten Windows.</span><span class="sxs-lookup"><span data-stu-id="4f507-146">The associated activity windows are highlighted in the Activity Windows list.</span></span>
* <span data-ttu-id="4f507-147">Egenskaper för det markerade objektet visas i fönstret Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="4f507-147">The properties of the selected object are shown in the Properties window.</span></span>

<span data-ttu-id="4f507-148">När pipeline aktiveras (inte i ett pausat tillstånd), visas det med en grön linje:</span><span class="sxs-lookup"><span data-stu-id="4f507-148">When the pipeline is enabled (not in a paused state), it's shown with a green line:</span></span>

![Pipelinen körs](./media/data-factory-monitor-manage-app/PipelineRunning.png)

<span data-ttu-id="4f507-150">Du kan pausa, återuppta eller avsluta en pipeline genom att markera den i diagramvyn och med hjälp av knapparna i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="4f507-150">You can pause, resume, or terminate a pipeline by selecting it in the diagram view and using the buttons on the command bar.</span></span>

![Pausa i kommandofältet](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
<span data-ttu-id="4f507-152">Det finns tre knapparna i fältet för pipeline i diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="4f507-152">There are three command bar buttons for the pipeline in the Diagram View.</span></span> <span data-ttu-id="4f507-153">Du kan använda den andra knappen för att pausa pipeline.</span><span class="sxs-lookup"><span data-stu-id="4f507-153">You can use the second button to pause the pipeline.</span></span> <span data-ttu-id="4f507-154">Pausa Avsluta inte pågående aktiviteter och kan fortsätta att slutföras.</span><span class="sxs-lookup"><span data-stu-id="4f507-154">Pausing doesn't terminate the currently running activities and lets them proceed to completion.</span></span> <span data-ttu-id="4f507-155">Knappen tredje pausar pipeline och avslutar sin befintliga aktiviteter körs.</span><span class="sxs-lookup"><span data-stu-id="4f507-155">The third button pauses the pipeline and terminates its existing executing activities.</span></span> <span data-ttu-id="4f507-156">Knappen första återupptar pipeline.</span><span class="sxs-lookup"><span data-stu-id="4f507-156">The first button resumes the pipeline.</span></span> <span data-ttu-id="4f507-157">När din pipeline pausas ändras färgen för pipeline.</span><span class="sxs-lookup"><span data-stu-id="4f507-157">When your pipeline is paused, the color of the pipeline changes.</span></span> <span data-ttu-id="4f507-158">Till exempel en pausad pipeline ser ut som i följande bild:</span><span class="sxs-lookup"><span data-stu-id="4f507-158">For example, a paused pipeline looks like in the following image:</span></span> 

![Pipeline pausats](./media/data-factory-monitor-manage-app/PipelinePaused.png)

<span data-ttu-id="4f507-160">Du kan välja flera två eller flera pipelines med Ctrl-tangenten.</span><span class="sxs-lookup"><span data-stu-id="4f507-160">You can multi-select two or more pipelines by using the Ctrl key.</span></span> <span data-ttu-id="4f507-161">Du kan använda knapparna i kommandofältet för att pausa/Fortsätt flera pipelines i taget.</span><span class="sxs-lookup"><span data-stu-id="4f507-161">You can use the command bar buttons to pause/resume multiple pipelines at a time.</span></span>

<span data-ttu-id="4f507-162">Du kan också högerklicka på en pipeline och välja alternativ för att pausa, fortsätta eller avsluta en pipeline.</span><span class="sxs-lookup"><span data-stu-id="4f507-162">You can also right-click a pipeline and select options to suspend, resume, or terminate a pipeline.</span></span> 

![Snabbmenyn för pipeline](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

<span data-ttu-id="4f507-164">Klicka på den **öppna pipeline** alternativet för att visa alla aktiviteter i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="4f507-164">Click the **Open pipeline** option to see all the activities in the pipeline.</span></span> 

![Menyn Öppna pipeline](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

<span data-ttu-id="4f507-166">I vyn öppnade pipeline kan du se alla aktiviteter i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="4f507-166">In the opened pipeline view, you see all activities in the pipeline.</span></span> <span data-ttu-id="4f507-167">I det här exemplet är bara en aktivitet: Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="4f507-167">In this example, there is only one activity: Copy Activity.</span></span> 

![Öppna pipeline](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

<span data-ttu-id="4f507-169">Om du vill gå tillbaka till den föregående vyn, klickar du på datafabriksnamnet i den dynamiska menyn längst upp.</span><span class="sxs-lookup"><span data-stu-id="4f507-169">To go back to the previous view, click the data factory name in the breadcrumb menu at the top.</span></span>

<span data-ttu-id="4f507-170">I pipeline-vyn när du väljer en utdatamängd eller när du flyttar musen över datamängd för utdata visas aktiviteten Windows popup-fönstret för denna dataset.</span><span class="sxs-lookup"><span data-stu-id="4f507-170">In the pipeline view, when you select an output dataset or when you move your mouse over the output dataset, you see the Activity Windows pop-up window for that dataset.</span></span>

![Aktiviteten Windows popup-fönster](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

<span data-ttu-id="4f507-172">Du kan klicka på ett fönster i aktiviteten för att se detaljer för den i den **egenskaper** fönster i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="4f507-172">You can click an activity window to see details for it in the **Properties** window in the right pane.</span></span>

![Fönstret Egenskaper för aktivitet](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

<span data-ttu-id="4f507-174">Växla till i den högra rutan i **aktivitet fönstret Explorer** fliken för att se mer information.</span><span class="sxs-lookup"><span data-stu-id="4f507-174">In the right pane, switch to the **Activity Window Explorer** tab to see more details.</span></span>

![Aktiviteten fönstret Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

<span data-ttu-id="4f507-176">Du också se **matcha variabler** för varje försök har misslyckats för en aktivitet i den **försök** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4f507-176">You also see **resolved variables** for each run attempt for an activity in the **Attempts** section.</span></span>

![Matcha variabler](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

<span data-ttu-id="4f507-178">Växla till den **skriptet** fliken för att se JSON-skript definitionen för det valda objektet.</span><span class="sxs-lookup"><span data-stu-id="4f507-178">Switch to the **Script** tab to see the JSON script definition for the selected object.</span></span>   

![Fliken skript](./media/data-factory-monitor-manage-app/ScriptTab.png)

<span data-ttu-id="4f507-180">Du kan se aktiviteten windows på tre platser:</span><span class="sxs-lookup"><span data-stu-id="4f507-180">You can see activity windows in three places:</span></span>

* <span data-ttu-id="4f507-181">Aktiviteten Windows popup-fönstret i diagramvyn (mellersta rutan).</span><span class="sxs-lookup"><span data-stu-id="4f507-181">The Activity Windows pop-up in the Diagram View (middle pane).</span></span>
* <span data-ttu-id="4f507-182">Aktiviteten fönstret Utforskaren i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="4f507-182">The Activity Window Explorer in the right pane.</span></span>
* <span data-ttu-id="4f507-183">Listan över aktiviteten Windows längst ned i fönstret.</span><span class="sxs-lookup"><span data-stu-id="4f507-183">The Activity Windows list in the bottom pane.</span></span>

<span data-ttu-id="4f507-184">Aktiviteten Windows popup-fönster och aktivitet Windows Explorer, kan du bläddra till i föregående vecka och nästa vecka med hjälp av vänster och höger pilarna.</span><span class="sxs-lookup"><span data-stu-id="4f507-184">In the Activity Windows pop-up and Activity Window Explorer, you can scroll to the previous week and the next week by using the left and right arrows.</span></span>

![Aktiviteten fönstret Explorer åt vänster och höger pilarna](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

<span data-ttu-id="4f507-186">Längst ned i diagramvyn visas dessa knappar: Zooma In, Zooma ut Zooma till innehåll, Zooma 100% Lås layout.</span><span class="sxs-lookup"><span data-stu-id="4f507-186">At the bottom of the Diagram View, you see these buttons: Zoom In, Zoom Out, Zoom to Fit, Zoom 100%, Lock layout.</span></span> <span data-ttu-id="4f507-187">Den **Lås layout** knappen förhindrar du av misstag flyttar tabeller och rörledningar i diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="4f507-187">The **Lock layout** button prevents you from accidentally moving tables and pipelines in the Diagram View.</span></span> <span data-ttu-id="4f507-188">Den är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="4f507-188">It's on by default.</span></span> <span data-ttu-id="4f507-189">Du kan stänga av den och flytta entiteter i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="4f507-189">You can turn it off and move entities around in the diagram.</span></span> <span data-ttu-id="4f507-190">När du inaktiverar den kan använda du knappen sista automatisk placering tabeller och rörledningar.</span><span class="sxs-lookup"><span data-stu-id="4f507-190">When you turn it off, you can use the last button to automatically position tables and pipelines.</span></span> <span data-ttu-id="4f507-191">Du kan zooma in eller ut genom att använda mushjulet.</span><span class="sxs-lookup"><span data-stu-id="4f507-191">You can also zoom in or out by using the mouse wheel.</span></span>

![Diagram visa zoomning kommandon](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a><span data-ttu-id="4f507-193">Lista över Windows-aktivitet</span><span class="sxs-lookup"><span data-stu-id="4f507-193">Activity Windows list</span></span>
<span data-ttu-id="4f507-194">Listan över aktiviteten Windows längst ned i den mellersta rutan visar alla aktivitetsfönster för datamängden som du valde i Resursläsaren eller diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="4f507-194">The Activity Windows list at the bottom of the middle pane displays all activity windows for the dataset that you selected in the Resource Explorer or the Diagram View.</span></span> <span data-ttu-id="4f507-195">Som standard är listan i fallande ordning, vilket innebär att du ser den senaste aktivitetsfönstret längst upp.</span><span class="sxs-lookup"><span data-stu-id="4f507-195">By default, the list is in descending order, which means that you see the latest activity window at the top.</span></span>

![Lista över Windows-aktivitet](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

<span data-ttu-id="4f507-197">Den här listan inte automatiskt, så Använd uppdateringsknappen i verktygsfältet manuellt uppdatera det.</span><span class="sxs-lookup"><span data-stu-id="4f507-197">This list doesn't refresh automatically, so use the refresh button on the toolbar to manually refresh it.</span></span>  

<span data-ttu-id="4f507-198">Aktiviteten windows kan vara i något av följande status:</span><span class="sxs-lookup"><span data-stu-id="4f507-198">Activity windows can be in one of the following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="4f507-199">Status</span><span class="sxs-lookup"><span data-stu-id="4f507-199">Status</span></span></th><th align="left"><span data-ttu-id="4f507-200">Substatus</span><span class="sxs-lookup"><span data-stu-id="4f507-200">Substatus</span></span></th><th align="left"><span data-ttu-id="4f507-201">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4f507-201">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="4f507-202">Väntar</span><span class="sxs-lookup"><span data-stu-id="4f507-202">Waiting</span></span></td><td><span data-ttu-id="4f507-203">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="4f507-203">ScheduleTime</span></span></td><td><span data-ttu-id="4f507-204">Tiden har inte inne för aktivitetsfönstret ska köras.</span><span class="sxs-lookup"><span data-stu-id="4f507-204">The time hasn't come for the activity window to run.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4f507-205">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="4f507-205">DatasetDependencies</span></span></td><td><span data-ttu-id="4f507-206">Uppströmsberoendena är inte redo.</span><span class="sxs-lookup"><span data-stu-id="4f507-206">The upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4f507-207">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="4f507-207">ComputeResources</span></span></td><td><span data-ttu-id="4f507-208">Beräkningsresurserna är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="4f507-208">The compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4f507-209">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="4f507-209">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="4f507-210">Alla aktivitetsinstanserna är upptagna med andra windows aktivitet.</span><span class="sxs-lookup"><span data-stu-id="4f507-210">All the activity instances are busy running other activity windows.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4f507-211">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="4f507-211">ActivityResume</span></span></td><td><span data-ttu-id="4f507-212">Aktiviteten har pausats och kan inte köra aktiviteten windows förrän den har återupptagits.</span><span class="sxs-lookup"><span data-stu-id="4f507-212">The activity is paused and can't run the activity windows until it's resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4f507-213">Försök igen</span><span class="sxs-lookup"><span data-stu-id="4f507-213">Retry</span></span></td><td><span data-ttu-id="4f507-214">Aktivitetskörningen försöks.</span><span class="sxs-lookup"><span data-stu-id="4f507-214">The activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4f507-215">Validering</span><span class="sxs-lookup"><span data-stu-id="4f507-215">Validation</span></span></td><td><span data-ttu-id="4f507-216">Verifieringen har inte startat ännu.</span><span class="sxs-lookup"><span data-stu-id="4f507-216">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4f507-217">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="4f507-217">ValidationRetry</span></span></td><td><span data-ttu-id="4f507-218">Verifieringen väntar på att göras.</span><span class="sxs-lookup"><span data-stu-id="4f507-218">Validation is waiting to be retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="4f507-219">InProgress</span><span class="sxs-lookup"><span data-stu-id="4f507-219">InProgress</span></span></td><td><span data-ttu-id="4f507-220">Verifiera</span><span class="sxs-lookup"><span data-stu-id="4f507-220">Validating</span></span></td><td><span data-ttu-id="4f507-221">Verifiering pågår.</span><span class="sxs-lookup"><span data-stu-id="4f507-221">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="4f507-222">Aktivitetsfönstret bearbetas.</span><span class="sxs-lookup"><span data-stu-id="4f507-222">The activity window is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="4f507-223">Det gick inte</span><span class="sxs-lookup"><span data-stu-id="4f507-223">Failed</span></span></td><td><span data-ttu-id="4f507-224">För lång tid</span><span class="sxs-lookup"><span data-stu-id="4f507-224">TimedOut</span></span></td><td><span data-ttu-id="4f507-225">Aktivitetskörningen tog längre tid än vad som tillåts av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="4f507-225">The activity execution took longer than what is allowed by the activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4f507-226">Avbrutna</span><span class="sxs-lookup"><span data-stu-id="4f507-226">Canceled</span></span></td><td><span data-ttu-id="4f507-227">Aktivitetsfönstret avbröts av en användare.</span><span class="sxs-lookup"><span data-stu-id="4f507-227">The activity window was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4f507-228">Validering</span><span class="sxs-lookup"><span data-stu-id="4f507-228">Validation</span></span></td><td><span data-ttu-id="4f507-229">Verifieringen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="4f507-229">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="4f507-230">Det gick inte att genereras eller verifiera Aktivitetsfönstret.</span><span class="sxs-lookup"><span data-stu-id="4f507-230">The activity window failed to be generated or validated.</span></span></td>
</tr>
<td><span data-ttu-id="4f507-231">Redo</span><span class="sxs-lookup"><span data-stu-id="4f507-231">Ready</span></span></td><td>-</td><td><span data-ttu-id="4f507-232">Aktivitetsfönstret är redo för användning.</span><span class="sxs-lookup"><span data-stu-id="4f507-232">The activity window is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4f507-233">Hoppades över</span><span class="sxs-lookup"><span data-stu-id="4f507-233">Skipped</span></span></td><td>-</td><td><span data-ttu-id="4f507-234">Aktivitetsfönstret bearbetas inte.</span><span class="sxs-lookup"><span data-stu-id="4f507-234">The activity window wasn't processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="4f507-235">Ingen</span><span class="sxs-lookup"><span data-stu-id="4f507-235">None</span></span></td><td>-</td><td><span data-ttu-id="4f507-236">En aktivitetsfönstret brukade finnas med en annan status, men har återställts.</span><span class="sxs-lookup"><span data-stu-id="4f507-236">An activity window used to exist with a different status, but has been reset.</span></span></td>
</tr>
</table>


<span data-ttu-id="4f507-237">När du klickar på en aktivitetsfönstret i listan visas information om den i den **aktivitet Utforskaren** eller **egenskaper** fönstret till höger.</span><span class="sxs-lookup"><span data-stu-id="4f507-237">When you click an activity window in the list, you see details about it in the **Activity Windows Explorer** or the **Properties** window on the right.</span></span>

![Aktiviteten fönstret Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a><span data-ttu-id="4f507-239">Uppdatera aktiviteten windows</span><span class="sxs-lookup"><span data-stu-id="4f507-239">Refresh activity windows</span></span>
<span data-ttu-id="4f507-240">Informationen uppdateras inte automatiskt, så Använd uppdateringsknappen (andra knappen) i kommandofältet för att manuellt uppdatera listan med windows.</span><span class="sxs-lookup"><span data-stu-id="4f507-240">The details aren't automatically refreshed, so use the refresh button (the second button) on the command bar to manually refresh the activity windows list.</span></span>  

### <a name="properties-window"></a><span data-ttu-id="4f507-241">Egenskapsfönstret</span><span class="sxs-lookup"><span data-stu-id="4f507-241">Properties window</span></span>
<span data-ttu-id="4f507-242">Fönstret Egenskaper är i rutan längst till höger i appen övervakning och hantering.</span><span class="sxs-lookup"><span data-stu-id="4f507-242">The Properties window is in the right-most pane of the Monitoring and Management app.</span></span>

![Egenskapsfönstret](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

<span data-ttu-id="4f507-244">Egenskaper för objekt som du har valt i Resursläsaren (trädvyn), diagramvyn eller aktivitet Windows listan visas.</span><span class="sxs-lookup"><span data-stu-id="4f507-244">It displays properties for the item that you selected in the Resource Explorer (tree view), Diagram View, or Activity Windows list.</span></span>

### <a name="activity-window-explorer"></a><span data-ttu-id="4f507-245">Aktiviteten fönstret Explorer</span><span class="sxs-lookup"><span data-stu-id="4f507-245">Activity Window Explorer</span></span>
<span data-ttu-id="4f507-246">Den **aktivitet fönstret Explorer** fönstret är i rutan längst till höger i appen övervakning och hantering.</span><span class="sxs-lookup"><span data-stu-id="4f507-246">The **Activity Window Explorer** window is in the right-most pane of the Monitoring and Management app.</span></span> <span data-ttu-id="4f507-247">Den visar information om aktivitetsfönstret som du valde i popup-fönstret aktivitet Windows eller aktivitet Windows-lista.</span><span class="sxs-lookup"><span data-stu-id="4f507-247">It displays details about the activity window that you selected in the Activity Windows pop-up window or the Activity Windows list.</span></span>

![Aktiviteten fönstret Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

<span data-ttu-id="4f507-249">Du kan växla till en annan aktivitetsfönstret genom att klicka på kalendervyn överst.</span><span class="sxs-lookup"><span data-stu-id="4f507-249">You can switch to another activity window by clicking it in the calendar view at the top.</span></span> <span data-ttu-id="4f507-250">Du kan också använda vänstra pilen och höger pilknappar för längst upp för att se aktiviteten windows från föregående vecka eller nästa vecka.</span><span class="sxs-lookup"><span data-stu-id="4f507-250">You can also use the left arrow/right arrow buttons at the top to see activity windows from the previous week or the next week.</span></span>

<span data-ttu-id="4f507-251">Du kan använda knapparna längst ned i fönstret för att köra aktivitetsfönstret eller uppdatera information i fönstret.</span><span class="sxs-lookup"><span data-stu-id="4f507-251">You can use the toolbar buttons in the bottom pane to rerun the activity window or refresh the details in the pane.</span></span>

### <a name="script"></a><span data-ttu-id="4f507-252">Skript</span><span class="sxs-lookup"><span data-stu-id="4f507-252">Script</span></span>
<span data-ttu-id="4f507-253">Du kan använda den **skriptet** att visa JSON-definitionen av den valda Data Factory-entiteten (länkad tjänst, datauppsättningen eller pipeline).</span><span class="sxs-lookup"><span data-stu-id="4f507-253">You can use the **Script** tab to view the JSON definition of the selected Data Factory entity (linked service, dataset, or pipeline).</span></span>

![Fliken skript](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a><span data-ttu-id="4f507-255">Använd systemvyer</span><span class="sxs-lookup"><span data-stu-id="4f507-255">Use system views</span></span>
<span data-ttu-id="4f507-256">Övervakning och hantering av appen innehåller fördefinierade systemvyer (**senaste aktivitet windows**, **misslyckades aktivitet windows**, **pågående aktivitet windows**) som tillåter Du kan visa senaste/misslyckades/pågående aktivitet windows för din data factory.</span><span class="sxs-lookup"><span data-stu-id="4f507-256">The Monitoring and Management app includes pre-built system views (**Recent activity windows**, **Failed activity windows**, **In-Progress activity windows**) that allow you to view recent/failed/in-progress activity windows for your data factory.</span></span>

<span data-ttu-id="4f507-257">Växla till den **övervakning vyer** fliken till vänster genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="4f507-257">Switch to the **Monitoring Views** tab on the left by clicking it.</span></span>

![Övervaka vyer fliken](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

<span data-ttu-id="4f507-259">Det finns tre systemvyer som stöds.</span><span class="sxs-lookup"><span data-stu-id="4f507-259">Currently, there are three system views that are supported.</span></span> <span data-ttu-id="4f507-260">Välj ett alternativ för att visa den senaste aktiviteten windows, underkända aktiviteten windows eller pågående aktivitet windows i listan aktivitet Windows (längst ned i den mellersta rutan).</span><span class="sxs-lookup"><span data-stu-id="4f507-260">Select an option to see recent activity windows, failed activity windows, or in-progress activity windows in the Activity Windows list (at the bottom of the middle pane).</span></span>

<span data-ttu-id="4f507-261">När du väljer den **senaste aktivitet windows** alternativet visas alla aktivitetsfönster för senaste i fallande ordning efter den **tid för senaste försök**.</span><span class="sxs-lookup"><span data-stu-id="4f507-261">When you select the **Recent activity windows** option, you see all recent activity windows in descending order of the **last attempt time**.</span></span>

<span data-ttu-id="4f507-262">Du kan använda den **misslyckades aktivitet windows** vill se alla underkända aktiviteten windows i listan.</span><span class="sxs-lookup"><span data-stu-id="4f507-262">You can use the **Failed activity windows** view to see all failed activity windows in the list.</span></span> <span data-ttu-id="4f507-263">Välj en misslyckad aktivitetsfönstret i listan för att se information om den i den **egenskaper** fönster eller **aktivitet fönstret Explorer**.</span><span class="sxs-lookup"><span data-stu-id="4f507-263">Select a failed activity window in the list to see details about it in the **Properties** window or the **Activity Window Explorer**.</span></span> <span data-ttu-id="4f507-264">Du kan också hämta loggar för misslyckade Aktivitetsfönstret.</span><span class="sxs-lookup"><span data-stu-id="4f507-264">You can also download any logs for a failed activity window.</span></span>

## <a name="sort-and-filter-activity-windows"></a><span data-ttu-id="4f507-265">Sortera och filtrera aktiviteten windows</span><span class="sxs-lookup"><span data-stu-id="4f507-265">Sort and filter activity windows</span></span>
<span data-ttu-id="4f507-266">Ändra den **starttid** och **sluttiden** inställningar i kommandofältet filter aktivitet windows.</span><span class="sxs-lookup"><span data-stu-id="4f507-266">Change the **start time** and **end time** settings in the command bar to filter activity windows.</span></span> <span data-ttu-id="4f507-267">Klicka på knappen bredvid sluttid för att uppdatera listan med aktiviteten Windows när du har ändrat starttid och sluttid.</span><span class="sxs-lookup"><span data-stu-id="4f507-267">After you change the start time and end time, click the button next to the end time to refresh the Activity Windows list.</span></span>

![Start- och sluttider](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> <span data-ttu-id="4f507-269">Alla tider för närvarande i UTC-format i appen övervakning och hantering.</span><span class="sxs-lookup"><span data-stu-id="4f507-269">Currently, all times are in UTC format in the Monitoring and Management app.</span></span>
>
>

<span data-ttu-id="4f507-270">I den **aktivitet Windows lista**, klicka på namnet på en kolumn (till exempel: Status).</span><span class="sxs-lookup"><span data-stu-id="4f507-270">In the **Activity Windows list**, click the name of a column (for example: Status).</span></span>

![Kolumnen standardmenyn Windows lista](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

<span data-ttu-id="4f507-272">Du kan göra följande:</span><span class="sxs-lookup"><span data-stu-id="4f507-272">You can do the following:</span></span>

* <span data-ttu-id="4f507-273">Sortering i stigande ordning.</span><span class="sxs-lookup"><span data-stu-id="4f507-273">Sort in ascending order.</span></span>
* <span data-ttu-id="4f507-274">Sortering i fallande ordning.</span><span class="sxs-lookup"><span data-stu-id="4f507-274">Sort in descending order.</span></span>
* <span data-ttu-id="4f507-275">Filtrera efter ett eller flera värden (klar, väntande och så vidare).</span><span class="sxs-lookup"><span data-stu-id="4f507-275">Filter by one or more values (Ready, Waiting, and so on).</span></span>

<span data-ttu-id="4f507-276">När du anger ett filter för en kolumn, kan du se filterknappen har aktiverats för den kolumn som visar att värdena i kolumnen filtrerade värdena.</span><span class="sxs-lookup"><span data-stu-id="4f507-276">When you specify a filter on a column, you see the filter button enabled for that column, which indicates that the values in the column are filtered values.</span></span>

![Filtrera efter en kolumn i listan över Windows-aktivitet](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

<span data-ttu-id="4f507-278">Du kan använda samma popup-fönstret för att ta bort filter.</span><span class="sxs-lookup"><span data-stu-id="4f507-278">You can use the same pop-up window to clear filters.</span></span> <span data-ttu-id="4f507-279">Klicka på knappen Ta bort filter i kommandofältet om du vill ta bort alla filter för aktiviteten Windows-listan.</span><span class="sxs-lookup"><span data-stu-id="4f507-279">To clear all filters for the Activity Windows list, click the clear filter button on the command bar.</span></span>

![Ta bort alla filter för aktiviteten Windows-lista](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a><span data-ttu-id="4f507-281">Utföra åtgärder för batch</span><span class="sxs-lookup"><span data-stu-id="4f507-281">Perform batch actions</span></span>
### <a name="rerun-selected-activity-windows"></a><span data-ttu-id="4f507-282">Kör vald aktivitet windows</span><span class="sxs-lookup"><span data-stu-id="4f507-282">Rerun selected activity windows</span></span>
<span data-ttu-id="4f507-283">Välj ett fönster med en aktivitet, klickar du på nedpilen för knappen första fältet och väljer **kör** / **kör med uppströms i pipeline**.</span><span class="sxs-lookup"><span data-stu-id="4f507-283">Select an activity window, click the down arrow for the first command bar button, and select **Rerun** / **Rerun with upstream in pipeline**.</span></span> <span data-ttu-id="4f507-284">När du väljer den **kör med uppströms i pipeline** alternativet den kör alla uppströmsaktivitet windows samt.</span><span class="sxs-lookup"><span data-stu-id="4f507-284">When you select the **Rerun with upstream in pipeline** option, it reruns all upstream activity windows as well.</span></span>
    <span data-ttu-id="4f507-285">![Kör en aktivitetsfönstret](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span><span class="sxs-lookup"><span data-stu-id="4f507-285">![Rerun an activity window](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span></span>

<span data-ttu-id="4f507-286">Du kan också markera flera aktivitetsfönster i listan och köra dem på samma gång.</span><span class="sxs-lookup"><span data-stu-id="4f507-286">You can also select multiple activity windows in the list and rerun them at the same time.</span></span> <span data-ttu-id="4f507-287">Du kanske vill filtrera aktiviteten windows baserat på status (till exempel: **misslyckades**)-- och kör sedan den underkända aktivitet windows när du har korrigerat problemet som orsakar aktivitet windows misslyckas.</span><span class="sxs-lookup"><span data-stu-id="4f507-287">You might want to filter activity windows based on the status (for example: **Failed**)--and then rerun the failed activity windows after correcting the issue that causes the activity windows to fail.</span></span> <span data-ttu-id="4f507-288">Se följande avsnitt för mer information om filtrering aktivitet windows i listan.</span><span class="sxs-lookup"><span data-stu-id="4f507-288">See the following section for details about filtering activity windows in the list.</span></span>  

### <a name="pauseresume-multiple-pipelines"></a><span data-ttu-id="4f507-289">Pausa/Fortsätt flera pipelines</span><span class="sxs-lookup"><span data-stu-id="4f507-289">Pause/resume multiple pipelines</span></span>
<span data-ttu-id="4f507-290">Du kan multiselect två eller flera pipelines med Ctrl-tangenten.</span><span class="sxs-lookup"><span data-stu-id="4f507-290">You can multiselect two or more pipelines by using the Ctrl key.</span></span> <span data-ttu-id="4f507-291">Du kan använda knapparna i kommandofältet (som är markerade i den röda rektangeln i följande bild) för att pausa/Fortsätt dem.</span><span class="sxs-lookup"><span data-stu-id="4f507-291">You can use the command bar buttons (which are highlighted in the red rectangle in the following image) to pause/resume them.</span></span>

![Pausa i kommandofältet](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="create-alerts"></a><span data-ttu-id="4f507-293">Skapa aviseringar</span><span class="sxs-lookup"><span data-stu-id="4f507-293">Create alerts</span></span>
<span data-ttu-id="4f507-294">Den **aviseringar** sidan kan du skapa en avisering och visa/redigera/ta bort befintliga aviseringar.</span><span class="sxs-lookup"><span data-stu-id="4f507-294">The **Alerts** page lets you create an alert and view/edit/delete existing alerts.</span></span> <span data-ttu-id="4f507-295">Du kan också inaktivera/aktivera en avisering.</span><span class="sxs-lookup"><span data-stu-id="4f507-295">You can also disable/enable an alert.</span></span> <span data-ttu-id="4f507-296">Klicka för att visa sidan aviseringar i **aviseringar** fliken.</span><span class="sxs-lookup"><span data-stu-id="4f507-296">To see the Alerts page, click the **Alerts** tab.</span></span>

![Aviseringsfliken](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a><span data-ttu-id="4f507-298">Så här skapar du en avisering</span><span class="sxs-lookup"><span data-stu-id="4f507-298">To create an alert</span></span>
1. <span data-ttu-id="4f507-299">Klicka på **Lägg till avisering** att lägga till en avisering.</span><span class="sxs-lookup"><span data-stu-id="4f507-299">Click **Add Alert** to add an alert.</span></span> <span data-ttu-id="4f507-300">Du ser den **information** sidan.</span><span class="sxs-lookup"><span data-stu-id="4f507-300">You see the **Details** page.</span></span>

    ![Skapa aviseringar - sidan](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. <span data-ttu-id="4f507-302">Ange den **namn** och **beskrivning** avisering och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="4f507-302">Specify the **Name** and **Description** for the alert, and click **Next**.</span></span> <span data-ttu-id="4f507-303">Du bör se den **filter** sidan.</span><span class="sxs-lookup"><span data-stu-id="4f507-303">You should see the **Filters** page.</span></span>

    ![Skapa aviseringar - sida](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. <span data-ttu-id="4f507-305">Välj den **händelse**, **status**, och **substatus** (valfritt) som du vill skapa en Data Factory-tjänsten för och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="4f507-305">Select the **event**, **status**, and **substatus** (optional) that you want to create a Data Factory service alert for, and click **Next**.</span></span> <span data-ttu-id="4f507-306">Du bör se den **mottagare** sidan.</span><span class="sxs-lookup"><span data-stu-id="4f507-306">You should see the **Recipients** page.</span></span>

    ![Skapa aviseringar - mottagare sida](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png)
4. <span data-ttu-id="4f507-308">Välj den **e-prenumerationsadministratörer** alternativet och/eller ange en **ytterligare administratör e-post**, och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="4f507-308">Select the **Email subscription admins** option and/or enter an **additional administrator email**, and click **Finish**.</span></span> <span data-ttu-id="4f507-309">Du bör se aviseringen i listan.</span><span class="sxs-lookup"><span data-stu-id="4f507-309">You should see the alert in the list.</span></span>

    ![Lista över aviseringar](./media/data-factory-monitor-manage-app/AlertsList.png)

<span data-ttu-id="4f507-311">Använd knapparna som är associerade med aviseringen för att redigera/ta bort/Aktiverar/inaktiverar en avisering i listan över aviseringar.</span><span class="sxs-lookup"><span data-stu-id="4f507-311">In the Alerts list, use the buttons that are associated with the alert to edit/delete/disable/enable an alert.</span></span>

### <a name="eventstatussubstatus"></a><span data-ttu-id="4f507-312">Substatus-händelse/status</span><span class="sxs-lookup"><span data-stu-id="4f507-312">Event/status/substatus</span></span>
<span data-ttu-id="4f507-313">Följande tabell innehåller en lista över tillgängliga händelser och status (och underordnad status).</span><span class="sxs-lookup"><span data-stu-id="4f507-313">The following table provides the list of available events and statuses (and substatuses).</span></span>

| <span data-ttu-id="4f507-314">händelsenamnet</span><span class="sxs-lookup"><span data-stu-id="4f507-314">Event name</span></span> | <span data-ttu-id="4f507-315">Status</span><span class="sxs-lookup"><span data-stu-id="4f507-315">Status</span></span> | <span data-ttu-id="4f507-316">Substatus</span><span class="sxs-lookup"><span data-stu-id="4f507-316">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4f507-317">Aktiviteten kör igång</span><span class="sxs-lookup"><span data-stu-id="4f507-317">Activity Run Started</span></span> |<span data-ttu-id="4f507-318">Igång</span><span class="sxs-lookup"><span data-stu-id="4f507-318">Started</span></span> |<span data-ttu-id="4f507-319">Startar</span><span class="sxs-lookup"><span data-stu-id="4f507-319">Starting</span></span> |
| <span data-ttu-id="4f507-320">Aktiviteten kör klar</span><span class="sxs-lookup"><span data-stu-id="4f507-320">Activity Run Finished</span></span> |<span data-ttu-id="4f507-321">Lyckades</span><span class="sxs-lookup"><span data-stu-id="4f507-321">Succeeded</span></span> |<span data-ttu-id="4f507-322">Lyckades</span><span class="sxs-lookup"><span data-stu-id="4f507-322">Succeeded</span></span> |
| <span data-ttu-id="4f507-323">Aktiviteten kör klar</span><span class="sxs-lookup"><span data-stu-id="4f507-323">Activity Run Finished</span></span> |<span data-ttu-id="4f507-324">Det gick inte</span><span class="sxs-lookup"><span data-stu-id="4f507-324">Failed</span></span> |<span data-ttu-id="4f507-325">Misslyckade resursallokering</span><span class="sxs-lookup"><span data-stu-id="4f507-325">Failed Resource Allocation</span></span><br/><br/><span data-ttu-id="4f507-326">Misslyckade körning</span><span class="sxs-lookup"><span data-stu-id="4f507-326">Failed Execution</span></span><br/><br/><span data-ttu-id="4f507-327">Tidsgränsen uppnåddes</span><span class="sxs-lookup"><span data-stu-id="4f507-327">Timed Out</span></span><br/><br/><span data-ttu-id="4f507-328">Inte kunde verifieras</span><span class="sxs-lookup"><span data-stu-id="4f507-328">Failed Validation</span></span><br/><br/><span data-ttu-id="4f507-329">Avbrutna</span><span class="sxs-lookup"><span data-stu-id="4f507-329">Abandoned</span></span> |
| <span data-ttu-id="4f507-330">På begäran HDI-klustret skapa igång</span><span class="sxs-lookup"><span data-stu-id="4f507-330">On-Demand HDI Cluster Create Started</span></span> |<span data-ttu-id="4f507-331">Igång</span><span class="sxs-lookup"><span data-stu-id="4f507-331">Started</span></span> |-|
| <span data-ttu-id="4f507-332">På begäran HDI-klustret har skapats</span><span class="sxs-lookup"><span data-stu-id="4f507-332">On-Demand HDI Cluster Created Successfully</span></span> |<span data-ttu-id="4f507-333">Lyckades</span><span class="sxs-lookup"><span data-stu-id="4f507-333">Succeeded</span></span> |-|
| <span data-ttu-id="4f507-334">På begäran HDI-klustret tas bort</span><span class="sxs-lookup"><span data-stu-id="4f507-334">On-Demand HDI Cluster Deleted</span></span> |<span data-ttu-id="4f507-335">Lyckades</span><span class="sxs-lookup"><span data-stu-id="4f507-335">Succeeded</span></span> |-|

### <a name="to-edit-delete-or-disable-an-alert"></a><span data-ttu-id="4f507-336">Om du vill redigera, ta bort eller inaktivera en avisering</span><span class="sxs-lookup"><span data-stu-id="4f507-336">To edit, delete, or disable an alert</span></span>

<span data-ttu-id="4f507-337">Använd följande knappar (markerat i rött) för att redigera, ta bort eller inaktivera en avisering.</span><span class="sxs-lookup"><span data-stu-id="4f507-337">Use the following buttons (highlighted in red) to edit, delete, or disable an alert.</span></span>

![Aviseringar knappar](./media/data-factory-monitor-manage-app/AlertButtons.png)
