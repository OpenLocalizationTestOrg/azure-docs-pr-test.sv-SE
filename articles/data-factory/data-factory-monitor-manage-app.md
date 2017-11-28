---
title: aaaMonitor och hantera data pipelines - Azure | Microsoft Docs
description: "Lär dig hur toouse hello övervakning och hantering av app toomonitor och hantera Azure datafabriker och rörledningar."
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
ms.openlocfilehash: 5e4ef6ec5fb8ebc9bda0be7899a39a51d58403d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-monitoring-and-management-app"></a><span data-ttu-id="2c6aa-103">Övervaka och hantera Azure Data Factory pipelines med hello övervakning och hantering av appen</span><span class="sxs-lookup"><span data-stu-id="2c6aa-103">Monitor and manage Azure Data Factory pipelines by using hello Monitoring and Management app</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2c6aa-104">Med hjälp av Azure portal/Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c6aa-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="2c6aa-105">Med hjälp av övervakning och Management-appen</span><span class="sxs-lookup"><span data-stu-id="2c6aa-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)
>
>

<span data-ttu-id="2c6aa-106">Den här artikeln beskriver hur toouse hello övervakning och hantering av app toomonitor, hantera och felsöka din Data Factory pipelines.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-106">This article describes how toouse hello Monitoring and Management app toomonitor, manage, and debug your Data Factory pipelines.</span></span> <span data-ttu-id="2c6aa-107">Det innehåller även information om hur toocreate aviseringar tooget meddelas när fel.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-107">It also provides information on how toocreate alerts tooget notified on failures.</span></span> <span data-ttu-id="2c6aa-108">Du kan komma igång med hjälp av programmet hello genom att titta på hello följande video:</span><span class="sxs-lookup"><span data-stu-id="2c6aa-108">You can get started with using hello application by watching hello following video:</span></span>

> [!NOTE]
> <span data-ttu-id="2c6aa-109">hello gränssnitt som visas i hello video kanske inte stämmer exakt vad som visas i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-109">hello user interface shown in hello video may not exactly match what you see in hello portal.</span></span> <span data-ttu-id="2c6aa-110">Det är något äldre men begrepp förblir hello samma.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-110">It's slightly older, but concepts remain hello same.</span></span> 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-hello-monitoring-and-management-app"></a><span data-ttu-id="2c6aa-111">Starta hello övervakning och hantering av appen</span><span class="sxs-lookup"><span data-stu-id="2c6aa-111">Launch hello Monitoring and Management app</span></span>
<span data-ttu-id="2c6aa-112">toolaunch hello Övervakare och Management-appen klickar du på hello **övervaka och hantera** panelen på hello **Data Factory** bladet för din data factory.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-112">toolaunch hello Monitor and Management app, click hello **Monitor & Manage** tile on hello **Data Factory** blade for your data factory.</span></span>

![Övervakning av panelen på startsidan för hello Data Factory](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

<span data-ttu-id="2c6aa-114">Du bör se hello övervakning och hantering av appen öppnas i ett separat fönster.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-114">You should see hello Monitoring and Management app open in a separate window.</span></span>  

![Övervaknings- och hanteringsapp](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> <span data-ttu-id="2c6aa-116">Om du ser att hello webbläsare har ”auktorisera...” Rensa hello **blockerar cookies från tredje part och platsdata** kryssrutan-- eller behålla den har markerats kan du skapa ett undantag för **login.microsoftonline.com** , och försök sedan tooopen hello appen igen.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-116">If you see that hello web browser is stuck at "Authorizing...", clear hello **Block third-party cookies and site data** check box--or keep it selected, create an exception for **login.microsoftonline.com**, and then try tooopen hello app again.</span></span>


<span data-ttu-id="2c6aa-117">I hello aktivitet Windows lista i hello mittenrutan ser du en aktivitetsfönstret för varje körning av en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-117">In hello Activity Windows list in hello middle pane, you see an activity window for each run of an activity.</span></span> <span data-ttu-id="2c6aa-118">Om du har varje timme hello schemalagd aktivitet toorun för fem timmar finns till exempel fem aktivitetsfönster som är associerade med fem datasektorer.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-118">For example, if you have hello activity scheduled toorun hourly for five hours, you see five activity windows associated with five data slices.</span></span> <span data-ttu-id="2c6aa-119">Om du inte ser aktivitet windows hello listan längst ned hello hello följande:</span><span class="sxs-lookup"><span data-stu-id="2c6aa-119">If you don't see activity windows in hello list at hello bottom, do hello following:</span></span>
 
- <span data-ttu-id="2c6aa-120">Uppdatera hello **starttid** och **sluttiden** filter på hello översta toomatch hello starta sluttider för din pipeline, och klicka sedan på hello **tillämpa** knappen.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-120">Update hello **start time** and **end time** filters at hello top toomatch hello start and end times of your pipeline, and then click hello **Apply** button.</span></span>  
- <span data-ttu-id="2c6aa-121">hello aktivitet Windows listan uppdateras inte automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-121">hello Activity Windows list is not automatically refreshed.</span></span> <span data-ttu-id="2c6aa-122">Klicka på hello **uppdatera** i verktygsfältet hello i hello **aktivitet Windows** lista.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-122">Click hello **Refresh** button on hello toolbar in hello **Activity Windows** list.</span></span>  

<span data-ttu-id="2c6aa-123">Om du inte har en Data Factory programmet tootest dessa steg med, hello Självstudier: [kopiera data från Blob Storage tooSQL databasen med hjälp av Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="2c6aa-123">If you don't have a Data Factory application tootest these steps with, do hello tutorial: [copy data from Blob Storage tooSQL Database using Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="understand-hello-monitoring-and-management-app"></a><span data-ttu-id="2c6aa-124">Förstå hello övervakning och hantering av appen</span><span class="sxs-lookup"><span data-stu-id="2c6aa-124">Understand hello Monitoring and Management app</span></span>
<span data-ttu-id="2c6aa-125">Det finns tre flikar hello vänster: **Resursläsaren**, **övervakning vyer**, och **aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-125">There are three tabs on hello left: **Resource Explorer**, **Monitoring Views**, and **Alerts**.</span></span> <span data-ttu-id="2c6aa-126">hello första fliken (**Resursläsaren**) väljs som standard.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-126">hello first tab (**Resource Explorer**) is selected by default.</span></span>

### <a name="resource-explorer"></a><span data-ttu-id="2c6aa-127">Resource Explorer</span><span class="sxs-lookup"><span data-stu-id="2c6aa-127">Resource Explorer</span></span>
<span data-ttu-id="2c6aa-128">Du ser hello följande:</span><span class="sxs-lookup"><span data-stu-id="2c6aa-128">You see hello following:</span></span>

* <span data-ttu-id="2c6aa-129">Hej Resursläsaren **trädvy** hello vänster.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-129">hello Resource Explorer **tree view** in hello left pane.</span></span>
* <span data-ttu-id="2c6aa-130">Hej **diagramvyn** hello överst i hello mellersta rutan.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-130">hello **Diagram View** at hello top in hello middle pane.</span></span>
* <span data-ttu-id="2c6aa-131">Hej **aktivitet Windows** lista längst ned hello i hello mellersta rutan.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-131">hello **Activity Windows** list at hello bottom in hello middle pane.</span></span>
* <span data-ttu-id="2c6aa-132">Hej **egenskaper**, **aktivitet fönstret Explorer**, och **skriptet** flikar i hello till höger.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-132">hello **Properties**, **Activity Window Explorer**, and **Script** tabs in hello right pane.</span></span>

<span data-ttu-id="2c6aa-133">I resursutforskaren visas alla resurser (pipelines, datauppsättningar, länkade tjänster) i hello data factory i en trädvy.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-133">In Resource Explorer, you see all resources (pipelines, datasets, linked services) in hello data factory in a tree view.</span></span> <span data-ttu-id="2c6aa-134">När du väljer ett objekt i Resursläsaren:</span><span class="sxs-lookup"><span data-stu-id="2c6aa-134">When you select an object in Resource Explorer:</span></span>

* <span data-ttu-id="2c6aa-135">hello associerade Data Factory entiteten är markerad i hello diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-135">hello associated Data Factory entity is highlighted in hello Diagram View.</span></span>
* <span data-ttu-id="2c6aa-136">[Associerade aktiviteten windows](data-factory-scheduling-and-execution.md) är markerade i hello aktivitet Windows lista längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-136">[Associated activity windows](data-factory-scheduling-and-execution.md) are highlighted in hello Activity Windows list at hello bottom.</span></span>  
* <span data-ttu-id="2c6aa-137">hello egenskaper för hello valda objekt visas i fönstret Egenskaper för hello i hello högra rutan.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-137">hello properties of hello selected object are shown in hello Properties window in hello right pane.</span></span>
* <span data-ttu-id="2c6aa-138">hello JSON-definitionen av hello valda objekt visas, om tillämpligt.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-138">hello JSON definition of hello selected object is shown, if applicable.</span></span> <span data-ttu-id="2c6aa-139">Exempel: en länkad tjänst, ett dataset eller en pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-139">For example: a linked service, a dataset, or a pipeline.</span></span>

![Resource Explorer](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

<span data-ttu-id="2c6aa-141">Se hello [schemaläggning och körning](data-factory-scheduling-and-execution.md) artikel detaljerad konceptuell information om aktiviteten windows.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-141">See hello [Scheduling and Execution](data-factory-scheduling-and-execution.md) article for detailed conceptual information about activity windows.</span></span>

### <a name="diagram-view"></a><span data-ttu-id="2c6aa-142">Diagramvy</span><span class="sxs-lookup"><span data-stu-id="2c6aa-142">Diagram View</span></span>
<span data-ttu-id="2c6aa-143">hello diagramvy för en datafabrik innehåller en enda om toomonitor och hantera en datafabrik och dess tillgångar.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-143">hello Diagram View of a data factory provides a single pane of glass toomonitor and manage a data factory and its assets.</span></span> <span data-ttu-id="2c6aa-144">När du väljer en Data Factory-entitet (dataset/pipeline) i hello diagramvyn:</span><span class="sxs-lookup"><span data-stu-id="2c6aa-144">When you select a Data Factory entity (dataset/pipeline) in hello Diagram View:</span></span>

* <span data-ttu-id="2c6aa-145">hello data factory-entiteten är markerad i hello trädvyn.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-145">hello data factory entity is selected in hello tree view.</span></span>
* <span data-ttu-id="2c6aa-146">hello associerade aktiviteten windows är markerade i hello aktivitet Windows lista.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-146">hello associated activity windows are highlighted in hello Activity Windows list.</span></span>
* <span data-ttu-id="2c6aa-147">hello egenskaper för hello valda objekt visas i fönstret Egenskaper för hello.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-147">hello properties of hello selected object are shown in hello Properties window.</span></span>

<span data-ttu-id="2c6aa-148">När hello pipeline aktiveras (inte i ett pausat tillstånd), visas det med en grön linje:</span><span class="sxs-lookup"><span data-stu-id="2c6aa-148">When hello pipeline is enabled (not in a paused state), it's shown with a green line:</span></span>

![Pipelinen körs](./media/data-factory-monitor-manage-app/PipelineRunning.png)

<span data-ttu-id="2c6aa-150">Du kan pausa, återuppta eller avsluta en pipeline genom att markera den i hello diagramvyn och hello knapparna i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-150">You can pause, resume, or terminate a pipeline by selecting it in hello diagram view and using hello buttons on hello command bar.</span></span>

![Pausa i hello kommandofält](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
<span data-ttu-id="2c6aa-152">Det finns tre knapparna i fältet för hello pipeline i hello diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-152">There are three command bar buttons for hello pipeline in hello Diagram View.</span></span> <span data-ttu-id="2c6aa-153">Du kan använda hello andra knappen toopause hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-153">You can use hello second button toopause hello pipeline.</span></span> <span data-ttu-id="2c6aa-154">Pausa Avsluta inte hello pågående aktiviteter och kan fortsätta toocompletion.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-154">Pausing doesn't terminate hello currently running activities and lets them proceed toocompletion.</span></span> <span data-ttu-id="2c6aa-155">hello tredje knappen pausar hello pipeline och avslutar sin befintliga aktiviteter körs.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-155">hello third button pauses hello pipeline and terminates its existing executing activities.</span></span> <span data-ttu-id="2c6aa-156">hello första knappen återupptar hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-156">hello first button resumes hello pipeline.</span></span> <span data-ttu-id="2c6aa-157">När din pipeline pausas ändras hello färgen för pipeline hello.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-157">When your pipeline is paused, hello color of hello pipeline changes.</span></span> <span data-ttu-id="2c6aa-158">Till exempel en pausad pipeline ser ut som i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="2c6aa-158">For example, a paused pipeline looks like in hello following image:</span></span> 

![Pipeline pausats](./media/data-factory-monitor-manage-app/PipelinePaused.png)

<span data-ttu-id="2c6aa-160">Du kan välja flera två eller flera pipelines med hello Ctrl-tangenten.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-160">You can multi-select two or more pipelines by using hello Ctrl key.</span></span> <span data-ttu-id="2c6aa-161">Du kan använda hello kommandot fältet knappar toopause/Fortsätt flera pipelines i taget.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-161">You can use hello command bar buttons toopause/resume multiple pipelines at a time.</span></span>

<span data-ttu-id="2c6aa-162">Du kan också högerklicka på en pipeline och välja alternativ för toosuspend återuppta eller avsluta en pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-162">You can also right-click a pipeline and select options toosuspend, resume, or terminate a pipeline.</span></span> 

![Snabbmenyn för pipeline](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

<span data-ttu-id="2c6aa-164">Klicka på hello **öppna pipeline** alternativet toosee alla hello aktiviteter i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-164">Click hello **Open pipeline** option toosee all hello activities in hello pipeline.</span></span> 

![Menyn Öppna pipeline](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

<span data-ttu-id="2c6aa-166">I hello öppnas pipeline vyn visas alla aktiviteter i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-166">In hello opened pipeline view, you see all activities in hello pipeline.</span></span> <span data-ttu-id="2c6aa-167">I det här exemplet är bara en aktivitet: Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-167">In this example, there is only one activity: Copy Activity.</span></span> 

![Öppna pipeline](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

<span data-ttu-id="2c6aa-169">toogo bakifrån toohello tidigare, klicka på hello datafabriksnamnet i hello dynamiska menyn hello överst.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-169">toogo back toohello previous view, click hello data factory name in hello breadcrumb menu at hello top.</span></span>

<span data-ttu-id="2c6aa-170">I hello pipeline vyn när du väljer en utdatamängd eller när du flyttar musen över hello utdatauppsättningen visas aktiviteten Windows hello popup-fönster för denna dataset.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-170">In hello pipeline view, when you select an output dataset or when you move your mouse over hello output dataset, you see hello Activity Windows pop-up window for that dataset.</span></span>

![Aktiviteten Windows popup-fönster](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

<span data-ttu-id="2c6aa-172">Du kan klicka på en aktivitet fönstret toosee information för den i hello **egenskaper** fönster i hello till höger.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-172">You can click an activity window toosee details for it in hello **Properties** window in hello right pane.</span></span>

![Fönstret Egenskaper för aktivitet](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

<span data-ttu-id="2c6aa-174">I högra fönstret hello växla toohello **aktivitet fönstret Explorer** fliken toosee mer information.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-174">In hello right pane, switch toohello **Activity Window Explorer** tab toosee more details.</span></span>

![Aktiviteten fönstret Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

<span data-ttu-id="2c6aa-176">Du också se **matcha variabler** för varje försök har misslyckats för en aktivitet i hello **försök** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-176">You also see **resolved variables** for each run attempt for an activity in hello **Attempts** section.</span></span>

![Matcha variabler](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

<span data-ttu-id="2c6aa-178">Växla toohello **skriptet** fliken toosee hello JSON-skript definitionen av hello valda objekt.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-178">Switch toohello **Script** tab toosee hello JSON script definition for hello selected object.</span></span>   

![Fliken skript](./media/data-factory-monitor-manage-app/ScriptTab.png)

<span data-ttu-id="2c6aa-180">Du kan se aktiviteten windows på tre platser:</span><span class="sxs-lookup"><span data-stu-id="2c6aa-180">You can see activity windows in three places:</span></span>

* <span data-ttu-id="2c6aa-181">hello aktivitet Windows popup i hello diagramvyn (mellersta rutan).</span><span class="sxs-lookup"><span data-stu-id="2c6aa-181">hello Activity Windows pop-up in hello Diagram View (middle pane).</span></span>
* <span data-ttu-id="2c6aa-182">hello aktivitet fönstret Explorer i hello till höger.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-182">hello Activity Window Explorer in hello right pane.</span></span>
* <span data-ttu-id="2c6aa-183">lista med hello aktivitet Windows hello längst ned i fönstret.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-183">hello Activity Windows list in hello bottom pane.</span></span>

<span data-ttu-id="2c6aa-184">Du kan rulla toohello föregående vecka i hello aktivitet Windows popup-fönster och aktivitet Windows Explorer, och hello hello nästa vecka med hjälp av vänster och höger pilarna.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-184">In hello Activity Windows pop-up and Activity Window Explorer, you can scroll toohello previous week and hello next week by using hello left and right arrows.</span></span>

![Aktiviteten fönstret Explorer åt vänster och höger pilarna](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

<span data-ttu-id="2c6aa-186">Längst ned hello hello diagramvyn, visas dessa knappar: Zooma In, Zooma ut, Zooma tooFit Zooma 100% Lås layout.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-186">At hello bottom of hello Diagram View, you see these buttons: Zoom In, Zoom Out, Zoom tooFit, Zoom 100%, Lock layout.</span></span> <span data-ttu-id="2c6aa-187">Hej **Lås layout** knappen förhindrar du av misstag flyttar tabeller och rörledningar i hello diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-187">hello **Lock layout** button prevents you from accidentally moving tables and pipelines in hello Diagram View.</span></span> <span data-ttu-id="2c6aa-188">Den är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-188">It's on by default.</span></span> <span data-ttu-id="2c6aa-189">Du kan stänga av den och flytta entiteter i hello diagram.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-189">You can turn it off and move entities around in hello diagram.</span></span> <span data-ttu-id="2c6aa-190">När du inaktiverar den kan använda du hello senaste knappen tooautomatically position tabeller och rörledningar.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-190">When you turn it off, you can use hello last button tooautomatically position tables and pipelines.</span></span> <span data-ttu-id="2c6aa-191">Du kan zooma in eller ut genom att använda hello mushjulet.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-191">You can also zoom in or out by using hello mouse wheel.</span></span>

![Diagram visa zoomning kommandon](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a><span data-ttu-id="2c6aa-193">Lista över Windows-aktivitet</span><span class="sxs-lookup"><span data-stu-id="2c6aa-193">Activity Windows list</span></span>
<span data-ttu-id="2c6aa-194">hello visas aktivitet Windows längst hello hello mellersta rutan alla aktiviteten windows hello dataset som du valde i hello Resursläsaren eller hello diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-194">hello Activity Windows list at hello bottom of hello middle pane displays all activity windows for hello dataset that you selected in hello Resource Explorer or hello Diagram View.</span></span> <span data-ttu-id="2c6aa-195">Som standard är hello listan i fallande ordning, vilket innebär att du ser hello senaste aktivitetsfönstret hello överst.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-195">By default, hello list is in descending order, which means that you see hello latest activity window at hello top.</span></span>

![Lista över Windows-aktivitet](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

<span data-ttu-id="2c6aa-197">Den här listan uppdateras inte automatiskt, så Använd hello uppdateringsknappen på hello verktygsfältet toomanually uppdatera den.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-197">This list doesn't refresh automatically, so use hello refresh button on hello toolbar toomanually refresh it.</span></span>  

<span data-ttu-id="2c6aa-198">Aktiviteten windows kan ha hello följande statusar:</span><span class="sxs-lookup"><span data-stu-id="2c6aa-198">Activity windows can be in one of hello following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="2c6aa-199">Status</span><span class="sxs-lookup"><span data-stu-id="2c6aa-199">Status</span></span></th><th align="left"><span data-ttu-id="2c6aa-200">Substatus</span><span class="sxs-lookup"><span data-stu-id="2c6aa-200">Substatus</span></span></th><th align="left"><span data-ttu-id="2c6aa-201">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2c6aa-201">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="2c6aa-202">Väntar</span><span class="sxs-lookup"><span data-stu-id="2c6aa-202">Waiting</span></span></td><td><span data-ttu-id="2c6aa-203">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="2c6aa-203">ScheduleTime</span></span></td><td><span data-ttu-id="2c6aa-204">hello tiden har inte inne för hello aktivitet fönstret toorun.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-204">hello time hasn't come for hello activity window toorun.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2c6aa-205">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="2c6aa-205">DatasetDependencies</span></span></td><td><span data-ttu-id="2c6aa-206">Hej uppströmsberoendena är inte redo.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-206">hello upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2c6aa-207">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="2c6aa-207">ComputeResources</span></span></td><td><span data-ttu-id="2c6aa-208">hello beräkningsresurser är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-208">hello compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2c6aa-209">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="2c6aa-209">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="2c6aa-210">Alla hello aktivitetsinstanserna är upptagna med andra windows aktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-210">All hello activity instances are busy running other activity windows.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2c6aa-211">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="2c6aa-211">ActivityResume</span></span></td><td><span data-ttu-id="2c6aa-212">hello aktiviteten har pausats och kan inte köras hello aktivitet windows förrän den har återupptagits.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-212">hello activity is paused and can't run hello activity windows until it's resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2c6aa-213">Försök igen</span><span class="sxs-lookup"><span data-stu-id="2c6aa-213">Retry</span></span></td><td><span data-ttu-id="2c6aa-214">Hej aktivitetskörningen försöks.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-214">hello activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2c6aa-215">Validering</span><span class="sxs-lookup"><span data-stu-id="2c6aa-215">Validation</span></span></td><td><span data-ttu-id="2c6aa-216">Verifieringen har inte startat ännu.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-216">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2c6aa-217">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="2c6aa-217">ValidationRetry</span></span></td><td><span data-ttu-id="2c6aa-218">Verifieringen är väntar toobe igen.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-218">Validation is waiting toobe retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="2c6aa-219">InProgress</span><span class="sxs-lookup"><span data-stu-id="2c6aa-219">InProgress</span></span></td><td><span data-ttu-id="2c6aa-220">Verifiera</span><span class="sxs-lookup"><span data-stu-id="2c6aa-220">Validating</span></span></td><td><span data-ttu-id="2c6aa-221">Verifiering pågår.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-221">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="2c6aa-222">hello aktivitetsfönstret bearbetas.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-222">hello activity window is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="2c6aa-223">Det gick inte</span><span class="sxs-lookup"><span data-stu-id="2c6aa-223">Failed</span></span></td><td><span data-ttu-id="2c6aa-224">För lång tid</span><span class="sxs-lookup"><span data-stu-id="2c6aa-224">TimedOut</span></span></td><td><span data-ttu-id="2c6aa-225">hello aktivitetskörningen tog längre tid än vad som tillåts av hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-225">hello activity execution took longer than what is allowed by hello activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2c6aa-226">Avbrutna</span><span class="sxs-lookup"><span data-stu-id="2c6aa-226">Canceled</span></span></td><td><span data-ttu-id="2c6aa-227">hello aktivitetsfönstret avbröts av en användare.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-227">hello activity window was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2c6aa-228">Validering</span><span class="sxs-lookup"><span data-stu-id="2c6aa-228">Validation</span></span></td><td><span data-ttu-id="2c6aa-229">Verifieringen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-229">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="2c6aa-230">Det gick inte att toobe genereras eller verifiera hello Aktivitetsfönstret.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-230">hello activity window failed toobe generated or validated.</span></span></td>
</tr>
<td><span data-ttu-id="2c6aa-231">Redo</span><span class="sxs-lookup"><span data-stu-id="2c6aa-231">Ready</span></span></td><td>-</td><td><span data-ttu-id="2c6aa-232">hello aktivitetsfönstret är redo för användning.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-232">hello activity window is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2c6aa-233">Hoppades över</span><span class="sxs-lookup"><span data-stu-id="2c6aa-233">Skipped</span></span></td><td>-</td><td><span data-ttu-id="2c6aa-234">hello aktivitetsfönstret bearbetas inte.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-234">hello activity window wasn't processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2c6aa-235">Ingen</span><span class="sxs-lookup"><span data-stu-id="2c6aa-235">None</span></span></td><td>-</td><td><span data-ttu-id="2c6aa-236">En aktivitetsfönstret för tooexist med en annan status, men har återställts.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-236">An activity window used tooexist with a different status, but has been reset.</span></span></td>
</tr>
</table>


<span data-ttu-id="2c6aa-237">När du klickar på en aktivitetsfönstret i hello listan kan du se information om det i hello **aktivitet Utforskaren** eller hello **egenskaper** hello högra fönstret.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-237">When you click an activity window in hello list, you see details about it in hello **Activity Windows Explorer** or hello **Properties** window on hello right.</span></span>

![Aktiviteten fönstret Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a><span data-ttu-id="2c6aa-239">Uppdatera aktiviteten windows</span><span class="sxs-lookup"><span data-stu-id="2c6aa-239">Refresh activity windows</span></span>
<span data-ttu-id="2c6aa-240">hello information uppdateras inte automatiskt, så hello uppdateringsknappen (hello andra) på hello i kommandofältet toomanually uppdatering hello aktivitet windows i listan.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-240">hello details aren't automatically refreshed, so use hello refresh button (hello second button) on hello command bar toomanually refresh hello activity windows list.</span></span>  

### <a name="properties-window"></a><span data-ttu-id="2c6aa-241">Egenskapsfönstret</span><span class="sxs-lookup"><span data-stu-id="2c6aa-241">Properties window</span></span>
<span data-ttu-id="2c6aa-242">hello egenskapsfönstret är hello längst till höger i fönstret hello övervakning och hantering av appen.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-242">hello Properties window is in hello right-most pane of hello Monitoring and Management app.</span></span>

![Egenskapsfönstret](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

<span data-ttu-id="2c6aa-244">Egenskaper för hello-objektet som du valde i hello Resursläsaren (trädvyn) visas diagramvyn eller aktivitet Windows lista.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-244">It displays properties for hello item that you selected in hello Resource Explorer (tree view), Diagram View, or Activity Windows list.</span></span>

### <a name="activity-window-explorer"></a><span data-ttu-id="2c6aa-245">Aktiviteten fönstret Explorer</span><span class="sxs-lookup"><span data-stu-id="2c6aa-245">Activity Window Explorer</span></span>
<span data-ttu-id="2c6aa-246">Hej **aktivitet fönstret Explorer** fönstret är hello längst till höger i fönstret hello övervakning och hantering av appen.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-246">hello **Activity Window Explorer** window is in hello right-most pane of hello Monitoring and Management app.</span></span> <span data-ttu-id="2c6aa-247">Information om hello aktivitetsfönstret som du valde i hello aktivitet Windows popup-fönster eller hello aktivitet Windows listan visas.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-247">It displays details about hello activity window that you selected in hello Activity Windows pop-up window or hello Activity Windows list.</span></span>

![Aktiviteten fönstret Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

<span data-ttu-id="2c6aa-249">Du kan växla tooanother aktivitetsfönstret genom att klicka på hello Kalender Visa hello överst.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-249">You can switch tooanother activity window by clicking it in hello calendar view at hello top.</span></span> <span data-ttu-id="2c6aa-250">Du kan också använda hello vänstra pilen och höger pilknappar för på hello översta toosee aktivitet windows från hello föregående vecka eller hello nästa vecka.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-250">You can also use hello left arrow/right arrow buttons at hello top toosee activity windows from hello previous week or hello next week.</span></span>

<span data-ttu-id="2c6aa-251">Du kan använda hello knappar i Aktivitetsfönstret för hello nedre fönstret toorerun hello eller uppdatera hello information hello i fönstret.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-251">You can use hello toolbar buttons in hello bottom pane toorerun hello activity window or refresh hello details in hello pane.</span></span>

### <a name="script"></a><span data-ttu-id="2c6aa-252">Skript</span><span class="sxs-lookup"><span data-stu-id="2c6aa-252">Script</span></span>
<span data-ttu-id="2c6aa-253">Du kan använda hello **skriptet** fliken tooview hello JSON-definitionen av hello valda Data Factory-enhet (länkad tjänst, datauppsättningen eller pipeline).</span><span class="sxs-lookup"><span data-stu-id="2c6aa-253">You can use hello **Script** tab tooview hello JSON definition of hello selected Data Factory entity (linked service, dataset, or pipeline).</span></span>

![Fliken skript](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a><span data-ttu-id="2c6aa-255">Använd systemvyer</span><span class="sxs-lookup"><span data-stu-id="2c6aa-255">Use system views</span></span>
<span data-ttu-id="2c6aa-256">hello övervakning och hantering av appen innehåller fördefinierade systemvyer (**senaste aktivitet windows**, **misslyckades aktivitet windows**, **pågående aktivitet windows**) som låter dig tooview senaste/misslyckades/pågående aktivitet windows för din data factory.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-256">hello Monitoring and Management app includes pre-built system views (**Recent activity windows**, **Failed activity windows**, **In-Progress activity windows**) that allow you tooview recent/failed/in-progress activity windows for your data factory.</span></span>

<span data-ttu-id="2c6aa-257">Växla toohello **övervakning vyer** fliken hello vänster genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-257">Switch toohello **Monitoring Views** tab on hello left by clicking it.</span></span>

![Övervaka vyer fliken](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

<span data-ttu-id="2c6aa-259">Det finns tre systemvyer som stöds.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-259">Currently, there are three system views that are supported.</span></span> <span data-ttu-id="2c6aa-260">Välj ett alternativ toosee senaste aktivitet windows, underkända aktiviteten windows eller pågående aktivitet windows hello aktivitet Windows listan (längst ned hello hello mellersta rutan).</span><span class="sxs-lookup"><span data-stu-id="2c6aa-260">Select an option toosee recent activity windows, failed activity windows, or in-progress activity windows in hello Activity Windows list (at hello bottom of hello middle pane).</span></span>

<span data-ttu-id="2c6aa-261">När du väljer hello **senaste aktivitet windows** alternativet visas alla aktivitetsfönster för senaste i fallande ordning efter hello **tid för senaste försök**.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-261">When you select hello **Recent activity windows** option, you see all recent activity windows in descending order of hello **last attempt time**.</span></span>

<span data-ttu-id="2c6aa-262">Du kan använda hello **misslyckades aktivitet windows** visa toosee alla misslyckades aktivitet windows hello-listan.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-262">You can use hello **Failed activity windows** view toosee all failed activity windows in hello list.</span></span> <span data-ttu-id="2c6aa-263">Välj en misslyckad aktivitetsfönstret i hello toosee information om det i hello **egenskaper** fönster eller hello **aktivitet fönstret Explorer**.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-263">Select a failed activity window in hello list toosee details about it in hello **Properties** window or hello **Activity Window Explorer**.</span></span> <span data-ttu-id="2c6aa-264">Du kan också hämta loggar för misslyckade Aktivitetsfönstret.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-264">You can also download any logs for a failed activity window.</span></span>

## <a name="sort-and-filter-activity-windows"></a><span data-ttu-id="2c6aa-265">Sortera och filtrera aktiviteten windows</span><span class="sxs-lookup"><span data-stu-id="2c6aa-265">Sort and filter activity windows</span></span>
<span data-ttu-id="2c6aa-266">Ändra hello **starttid** och **sluttiden** inställningar i hello i kommandofältet toofilter aktivitet windows.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-266">Change hello **start time** and **end time** settings in hello command bar toofilter activity windows.</span></span> <span data-ttu-id="2c6aa-267">Klicka på hello knappen Nästa toohello end tid toorefresh hello aktivitet Windows lista när du har ändrat hello starttid och sluttid.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-267">After you change hello start time and end time, click hello button next toohello end time toorefresh hello Activity Windows list.</span></span>

![Start- och sluttider](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> <span data-ttu-id="2c6aa-269">Alla tider för närvarande i UTC-format i hello övervakning och hantering av app.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-269">Currently, all times are in UTC format in hello Monitoring and Management app.</span></span>
>
>

<span data-ttu-id="2c6aa-270">I hello **aktivitet Windows lista**, klicka på hello namnet på en kolumn (till exempel: Status).</span><span class="sxs-lookup"><span data-stu-id="2c6aa-270">In hello **Activity Windows list**, click hello name of a column (for example: Status).</span></span>

![Kolumnen standardmenyn Windows lista](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

<span data-ttu-id="2c6aa-272">Du kan göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="2c6aa-272">You can do hello following:</span></span>

* <span data-ttu-id="2c6aa-273">Sortering i stigande ordning.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-273">Sort in ascending order.</span></span>
* <span data-ttu-id="2c6aa-274">Sortering i fallande ordning.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-274">Sort in descending order.</span></span>
* <span data-ttu-id="2c6aa-275">Filtrera efter ett eller flera värden (klar, väntande och så vidare).</span><span class="sxs-lookup"><span data-stu-id="2c6aa-275">Filter by one or more values (Ready, Waiting, and so on).</span></span>

<span data-ttu-id="2c6aa-276">Hello filterknappen aktiverad för den kolumn som visar att hello värdena i kolumnen hello filtrerade värden visas när du anger ett filter på en kolumn.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-276">When you specify a filter on a column, you see hello filter button enabled for that column, which indicates that hello values in hello column are filtered values.</span></span>

![Filtrera efter en kolumn på aktiviteten Windows hello](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

<span data-ttu-id="2c6aa-278">Du kan använda hello samma popup-fönster tooclear filter.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-278">You can use hello same pop-up window tooclear filters.</span></span> <span data-ttu-id="2c6aa-279">tooclear alla filtrerar hello aktivitet Windows lista Klicka hello Rensa filter i hello kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-279">tooclear all filters for hello Activity Windows list, click hello clear filter button on hello command bar.</span></span>

![Ta bort alla filter för hello aktivitet Windows lista](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a><span data-ttu-id="2c6aa-281">Utföra åtgärder för batch</span><span class="sxs-lookup"><span data-stu-id="2c6aa-281">Perform batch actions</span></span>
### <a name="rerun-selected-activity-windows"></a><span data-ttu-id="2c6aa-282">Kör vald aktivitet windows</span><span class="sxs-lookup"><span data-stu-id="2c6aa-282">Rerun selected activity windows</span></span>
<span data-ttu-id="2c6aa-283">Ett fönster med en aktivitet, klicka på hello NEDPIL för hello första fältet kommandoknapp och markera **kör** / **kör med uppströms i pipeline**.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-283">Select an activity window, click hello down arrow for hello first command bar button, and select **Rerun** / **Rerun with upstream in pipeline**.</span></span> <span data-ttu-id="2c6aa-284">När du väljer hello **kör med uppströms i pipeline** alternativet den kör alla uppströmsaktivitet windows samt.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-284">When you select hello **Rerun with upstream in pipeline** option, it reruns all upstream activity windows as well.</span></span>
    <span data-ttu-id="2c6aa-285">![Kör en aktivitetsfönstret](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span><span class="sxs-lookup"><span data-stu-id="2c6aa-285">![Rerun an activity window](./media/data-factory-monitor-manage-app/ReRunSlice.png)</span></span>

<span data-ttu-id="2c6aa-286">Du kan också Välj flera aktivitet windows hello listan och köra dem igen på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-286">You can also select multiple activity windows in hello list and rerun them at hello same time.</span></span> <span data-ttu-id="2c6aa-287">Du kanske vill toofilter aktivitet windows-baserade på hello status (till exempel: **misslyckades**)-- och kör sedan hello misslyckades aktivitet windows när du har korrigerat hello problemet som orsakar hello aktivitet windows toofail.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-287">You might want toofilter activity windows based on hello status (for example: **Failed**)--and then rerun hello failed activity windows after correcting hello issue that causes hello activity windows toofail.</span></span> <span data-ttu-id="2c6aa-288">Se följande information om filtrering aktivitet windows hello listan hello.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-288">See hello following section for details about filtering activity windows in hello list.</span></span>  

### <a name="pauseresume-multiple-pipelines"></a><span data-ttu-id="2c6aa-289">Pausa/Fortsätt flera pipelines</span><span class="sxs-lookup"><span data-stu-id="2c6aa-289">Pause/resume multiple pipelines</span></span>
<span data-ttu-id="2c6aa-290">Du kan multiselect två eller flera pipelines med hello Ctrl-tangenten.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-290">You can multiselect two or more pipelines by using hello Ctrl key.</span></span> <span data-ttu-id="2c6aa-291">Du kan använda hello knapparna i fältet (som är markerade i hello röd rektangel i följande bild hello) toopause/återuppta dem.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-291">You can use hello command bar buttons (which are highlighted in hello red rectangle in hello following image) toopause/resume them.</span></span>

![Pausa i hello kommandofält](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="create-alerts"></a><span data-ttu-id="2c6aa-293">Skapa aviseringar</span><span class="sxs-lookup"><span data-stu-id="2c6aa-293">Create alerts</span></span>
<span data-ttu-id="2c6aa-294">Hej **aviseringar** sidan kan du skapa en avisering och visa/redigera/ta bort befintliga aviseringar.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-294">hello **Alerts** page lets you create an alert and view/edit/delete existing alerts.</span></span> <span data-ttu-id="2c6aa-295">Du kan också inaktivera/aktivera en avisering.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-295">You can also disable/enable an alert.</span></span> <span data-ttu-id="2c6aa-296">toosee Hej sidan varningar, klicka på hello **aviseringar** fliken.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-296">toosee hello Alerts page, click hello **Alerts** tab.</span></span>

![Aviseringsfliken](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="toocreate-an-alert"></a><span data-ttu-id="2c6aa-298">toocreate en avisering</span><span class="sxs-lookup"><span data-stu-id="2c6aa-298">toocreate an alert</span></span>
1. <span data-ttu-id="2c6aa-299">Klicka på **Lägg till avisering** tooadd en avisering.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-299">Click **Add Alert** tooadd an alert.</span></span> <span data-ttu-id="2c6aa-300">Du ser hello **information** sidan.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-300">You see hello **Details** page.</span></span>

    ![Skapa aviseringar - sidan](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. <span data-ttu-id="2c6aa-302">Ange hello **namn** och **beskrivning** hello avisering och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-302">Specify hello **Name** and **Description** for hello alert, and click **Next**.</span></span> <span data-ttu-id="2c6aa-303">Du bör se hello **filter** sidan.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-303">You should see hello **Filters** page.</span></span>

    ![Skapa aviseringar - sida](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. <span data-ttu-id="2c6aa-305">Välj hello **händelse**, **status**, och **substatus** (valfritt) som du vill toocreate Data Factory-tjänsten avisering för och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-305">Select hello **event**, **status**, and **substatus** (optional) that you want toocreate a Data Factory service alert for, and click **Next**.</span></span> <span data-ttu-id="2c6aa-306">Du bör se hello **mottagare** sidan.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-306">You should see hello **Recipients** page.</span></span>

    ![Skapa aviseringar - mottagare sida](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png)
4. <span data-ttu-id="2c6aa-308">Välj hello **e-prenumerationsadministratörer** alternativet och/eller ange en **ytterligare administratör e-post**, och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-308">Select hello **Email subscription admins** option and/or enter an **additional administrator email**, and click **Finish**.</span></span> <span data-ttu-id="2c6aa-309">Du bör se hello avisering i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-309">You should see hello alert in hello list.</span></span>

    ![Lista över aviseringar](./media/data-factory-monitor-manage-app/AlertsList.png)

<span data-ttu-id="2c6aa-311">I hello aviseringslistan knapparna hello som är associerade med hello avisering tooedit/delete/Aktiverar/inaktiverar en avisering.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-311">In hello Alerts list, use hello buttons that are associated with hello alert tooedit/delete/disable/enable an alert.</span></span>

### <a name="eventstatussubstatus"></a><span data-ttu-id="2c6aa-312">Substatus-händelse/status</span><span class="sxs-lookup"><span data-stu-id="2c6aa-312">Event/status/substatus</span></span>
<span data-ttu-id="2c6aa-313">hello innehåller följande tabell hello lista över tillgängliga händelser och status (och underordnad status).</span><span class="sxs-lookup"><span data-stu-id="2c6aa-313">hello following table provides hello list of available events and statuses (and substatuses).</span></span>

| <span data-ttu-id="2c6aa-314">händelsenamnet</span><span class="sxs-lookup"><span data-stu-id="2c6aa-314">Event name</span></span> | <span data-ttu-id="2c6aa-315">Status</span><span class="sxs-lookup"><span data-stu-id="2c6aa-315">Status</span></span> | <span data-ttu-id="2c6aa-316">Substatus</span><span class="sxs-lookup"><span data-stu-id="2c6aa-316">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2c6aa-317">Aktiviteten kör igång</span><span class="sxs-lookup"><span data-stu-id="2c6aa-317">Activity Run Started</span></span> |<span data-ttu-id="2c6aa-318">Igång</span><span class="sxs-lookup"><span data-stu-id="2c6aa-318">Started</span></span> |<span data-ttu-id="2c6aa-319">Startar</span><span class="sxs-lookup"><span data-stu-id="2c6aa-319">Starting</span></span> |
| <span data-ttu-id="2c6aa-320">Aktiviteten kör klar</span><span class="sxs-lookup"><span data-stu-id="2c6aa-320">Activity Run Finished</span></span> |<span data-ttu-id="2c6aa-321">Lyckades</span><span class="sxs-lookup"><span data-stu-id="2c6aa-321">Succeeded</span></span> |<span data-ttu-id="2c6aa-322">Lyckades</span><span class="sxs-lookup"><span data-stu-id="2c6aa-322">Succeeded</span></span> |
| <span data-ttu-id="2c6aa-323">Aktiviteten kör klar</span><span class="sxs-lookup"><span data-stu-id="2c6aa-323">Activity Run Finished</span></span> |<span data-ttu-id="2c6aa-324">Det gick inte</span><span class="sxs-lookup"><span data-stu-id="2c6aa-324">Failed</span></span> |<span data-ttu-id="2c6aa-325">Misslyckade resursallokering</span><span class="sxs-lookup"><span data-stu-id="2c6aa-325">Failed Resource Allocation</span></span><br/><br/><span data-ttu-id="2c6aa-326">Misslyckade körning</span><span class="sxs-lookup"><span data-stu-id="2c6aa-326">Failed Execution</span></span><br/><br/><span data-ttu-id="2c6aa-327">Tidsgränsen uppnåddes</span><span class="sxs-lookup"><span data-stu-id="2c6aa-327">Timed Out</span></span><br/><br/><span data-ttu-id="2c6aa-328">Inte kunde verifieras</span><span class="sxs-lookup"><span data-stu-id="2c6aa-328">Failed Validation</span></span><br/><br/><span data-ttu-id="2c6aa-329">Avbrutna</span><span class="sxs-lookup"><span data-stu-id="2c6aa-329">Abandoned</span></span> |
| <span data-ttu-id="2c6aa-330">På begäran HDI-klustret skapa igång</span><span class="sxs-lookup"><span data-stu-id="2c6aa-330">On-Demand HDI Cluster Create Started</span></span> |<span data-ttu-id="2c6aa-331">Igång</span><span class="sxs-lookup"><span data-stu-id="2c6aa-331">Started</span></span> |-|
| <span data-ttu-id="2c6aa-332">På begäran HDI-klustret har skapats</span><span class="sxs-lookup"><span data-stu-id="2c6aa-332">On-Demand HDI Cluster Created Successfully</span></span> |<span data-ttu-id="2c6aa-333">Lyckades</span><span class="sxs-lookup"><span data-stu-id="2c6aa-333">Succeeded</span></span> |-|
| <span data-ttu-id="2c6aa-334">På begäran HDI-klustret tas bort</span><span class="sxs-lookup"><span data-stu-id="2c6aa-334">On-Demand HDI Cluster Deleted</span></span> |<span data-ttu-id="2c6aa-335">Lyckades</span><span class="sxs-lookup"><span data-stu-id="2c6aa-335">Succeeded</span></span> |-|

### <a name="tooedit-delete-or-disable-an-alert"></a><span data-ttu-id="2c6aa-336">tooedit, ta bort eller inaktivera en avisering</span><span class="sxs-lookup"><span data-stu-id="2c6aa-336">tooedit, delete, or disable an alert</span></span>

<span data-ttu-id="2c6aa-337">Använd hello följande knappar (markerat i rött) tooedit, ta bort eller inaktivera en avisering.</span><span class="sxs-lookup"><span data-stu-id="2c6aa-337">Use hello following buttons (highlighted in red) tooedit, delete, or disable an alert.</span></span>

![Aviseringar knappar](./media/data-factory-monitor-manage-app/AlertButtons.png)
