---
title: aaaMonitor och hantera pipelines med hello Azure portal och PowerShell | Microsoft Docs
description: "Lär dig hur toouse hello Azure-portalen och Azure PowerShell toomonitor och hantera hello Azure datafabriker och pipelines som du har skapat."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: a8d3c7943e79450895ff754f06a37fdad1cbef92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-azure-portal-and-powershell"></a><span data-ttu-id="aa4d3-103">Övervaka och hantera Azure Data Factory pipelines med hello Azure portal och PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa4d3-103">Monitor and manage Azure Data Factory pipelines by using hello Azure portal and PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="aa4d3-104">Med hjälp av Azure portal/Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa4d3-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="aa4d3-105">Med hjälp av övervakning och Management-appen</span><span class="sxs-lookup"><span data-stu-id="aa4d3-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)


> [!IMPORTANT]
> <span data-ttu-id="aa4d3-106">hello övervakning och hantering av programmet ger ett bättre stöd för att övervaka och hantera dina data pipelines och felsöka eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-106">hello monitoring & management application provides a better support for monitoring and managing your data pipelines, and troubleshooting any issues.</span></span> <span data-ttu-id="aa4d3-107">Mer information om hur du använder programmet hello finns [övervaka och hantera Data Factory pipelines genom att använda appen för övervakning och hantering av hello](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="aa4d3-107">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 


<span data-ttu-id="aa4d3-108">Den här artikeln beskrivs hur toomonitor, hantera och felsöka din pipelines med hjälp av Azure portal och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-108">This article describes how toomonitor, manage, and debug your pipelines by using Azure portal and PowerShell.</span></span> <span data-ttu-id="aa4d3-109">hello artikeln innehåller även information om hur toocreate aviseringar och få ett meddelande om misslyckade.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-109">hello article also provides information on how toocreate alerts and get notified about failures.</span></span>

## <a name="understand-pipelines-and-activity-states"></a><span data-ttu-id="aa4d3-110">Förstå pipelines och aktivitet tillstånd</span><span class="sxs-lookup"><span data-stu-id="aa4d3-110">Understand pipelines and activity states</span></span>
<span data-ttu-id="aa4d3-111">Med hjälp av hello Azure-portalen kan du:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-111">By using hello Azure portal, you can:</span></span>

* <span data-ttu-id="aa4d3-112">Visa din data factory som ett diagram.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-112">View your data factory as a diagram.</span></span>
* <span data-ttu-id="aa4d3-113">Visa aktiviteter i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-113">View activities in a pipeline.</span></span>
* <span data-ttu-id="aa4d3-114">Visa inkommande och utgående datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-114">View input and output datasets.</span></span>

<span data-ttu-id="aa4d3-115">Det här avsnittet beskriver också hur en datamängdssektor övergångar från ett tillstånd tooanother tillstånd.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-115">This section also describes how a dataset slice transitions from one state tooanother state.</span></span>   

### <a name="navigate-tooyour-data-factory"></a><span data-ttu-id="aa4d3-116">Navigera tooyour data factory</span><span class="sxs-lookup"><span data-stu-id="aa4d3-116">Navigate tooyour data factory</span></span>
1. <span data-ttu-id="aa4d3-117">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="aa4d3-117">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="aa4d3-118">Klicka på **datafabriker** på hello menyn hello vänster.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-118">Click **Data factories** on hello menu on hello left.</span></span> <span data-ttu-id="aa4d3-119">Om du inte ser det klickar du på **fler tjänster >**, och klicka sedan på **datafabriker** under hello **INTELLIGENCE + analys** kategori.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-119">If you don't see it, click **More services >**, and then click **Data factories** under hello **INTELLIGENCE + ANALYTICS** category.</span></span>

   ![Bläddra igenom alla > datafabriker](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. <span data-ttu-id="aa4d3-121">På hello **datafabriker** bladet, Välj hello data factory som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-121">On hello **Data factories** blade, select hello data factory that you're interested in.</span></span>

    ![Välj data factory](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   <span data-ttu-id="aa4d3-123">Du bör se hello startsidan för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-123">You should see hello home page for hello data factory.</span></span>

   ![Data factory-bladet](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a><span data-ttu-id="aa4d3-125">Diagramvy i din data factory</span><span class="sxs-lookup"><span data-stu-id="aa4d3-125">Diagram view of your data factory</span></span>
<span data-ttu-id="aa4d3-126">Hej **Diagram** vy av en datafabrik som innehåller en enda om toomonitor och hantera hello data factory och dess tillgångar.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-126">hello **Diagram** view of a data factory provides a single pane of glass toomonitor and manage hello data factory and its assets.</span></span> <span data-ttu-id="aa4d3-127">toosee hello **Diagram** visa i din data factory, klickar du på **Diagram** på hello startsida för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-127">toosee hello **Diagram** view of your data factory, click **Diagram** on hello home page for hello data factory.</span></span>

![Diagramvy](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

<span data-ttu-id="aa4d3-129">Du kan zooma in, Zooma in, Zooma toofit, Zooma too100%, lås hello layout hello diagram och automatiskt placera pipelines och datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-129">You can zoom in, zoom out, zoom toofit, zoom too100%, lock hello layout of hello diagram, and automatically position pipelines and datasets.</span></span> <span data-ttu-id="aa4d3-130">Du kan också se hello härkomst informationen (det vill säga visa överordnade och underordnade objekt av valda objekt).</span><span class="sxs-lookup"><span data-stu-id="aa4d3-130">You can also see hello data lineage information (that is, show upstream and downstream items of selected items).</span></span>

### <a name="activities-inside-a-pipeline"></a><span data-ttu-id="aa4d3-131">Aktiviteter i en pipeline</span><span class="sxs-lookup"><span data-stu-id="aa4d3-131">Activities inside a pipeline</span></span>
1. <span data-ttu-id="aa4d3-132">Högerklicka på hello pipeline och klicka sedan på **öppna pipeline** toosee alla aktiviteter i hello pipeline tillsammans med indata- och datauppsättningar för hello aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-132">Right-click hello pipeline, and then click **Open pipeline** toosee all activities in hello pipeline, along with input and output datasets for hello activities.</span></span> <span data-ttu-id="aa4d3-133">Den här funktionen är användbart när din pipeline innehåller mer än en aktivitet och du vill toounderstand hello operativa övriga en enkel rörledning.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-133">This feature is useful when your pipeline includes more than one activity and you want toounderstand hello operational lineage of a single pipeline.</span></span>

    ![Menyn Öppna pipeline](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. <span data-ttu-id="aa4d3-135">I följande exempel hello, ser du en kopia aktivitet i pipelinen hello med indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-135">In hello following example, you see a copy activity in hello pipeline with an input and an output.</span></span> 

    ![Aktiviteter i en pipeline](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. <span data-ttu-id="aa4d3-137">Du kan gå tillbaka toohello startsidan i hello data factory genom att klicka på hello **datafabriken** länken i hello spåret hello övre vänstra hörn.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-137">You can navigate back toohello home page of hello data factory by clicking hello **Data factory** link in hello breadcrumb at hello top-left corner.</span></span>

    ![Gå tillbaka toodata fabriken](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-hello-state-of-each-activity-inside-a-pipeline"></a><span data-ttu-id="aa4d3-139">Hello visningsstatus för varje aktivitet i en pipeline</span><span class="sxs-lookup"><span data-stu-id="aa4d3-139">View hello state of each activity inside a pipeline</span></span>
<span data-ttu-id="aa4d3-140">Du kan visa hello aktuell status för en aktivitet genom att visa hello status för hello datauppsättningar som produceras av hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-140">You can view hello current state of an activity by viewing hello status of any of hello datasets that are produced by hello activity.</span></span>

<span data-ttu-id="aa4d3-141">Genom att dubbelklicka på hello **OutputBlobTable** i hello **Diagram**, du kan se alla hello-segment som genereras av en annan aktivitet körs i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-141">By double-clicking hello **OutputBlobTable** in hello **Diagram**, you can see all hello slices that are produced by different activity runs inside a pipeline.</span></span> <span data-ttu-id="aa4d3-142">Du kan se att hello kopieringsaktiviteten för hello har körts senaste åtta timmar och producerade hello segment i hello **klar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-142">You can see that hello copy activity ran successfully for hello last eight hours and produced hello slices in hello **Ready** state.</span></span>  

![Tillstånd för hello pipeline](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

<span data-ttu-id="aa4d3-144">hello dataset segment i hello data factory kan ha något av följande statusar hello:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-144">hello dataset slices in hello data factory can have one of hello following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="aa4d3-145">Status</span><span class="sxs-lookup"><span data-stu-id="aa4d3-145">State</span></span></th><th align="left"><span data-ttu-id="aa4d3-146">Undertillstånden</span><span class="sxs-lookup"><span data-stu-id="aa4d3-146">Substate</span></span></th><th align="left"><span data-ttu-id="aa4d3-147">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="aa4d3-147">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="aa4d3-148">Väntar</span><span class="sxs-lookup"><span data-stu-id="aa4d3-148">Waiting</span></span></td><td><span data-ttu-id="aa4d3-149">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="aa4d3-149">ScheduleTime</span></span></td><td><span data-ttu-id="aa4d3-150">hello tiden har inte inne för hello sektorn toorun.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-150">hello time hasn't come for hello slice toorun.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="aa4d3-151">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="aa4d3-151">DatasetDependencies</span></span></td><td><span data-ttu-id="aa4d3-152">Hej uppströmsberoendena är inte redo.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-152">hello upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="aa4d3-153">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="aa4d3-153">ComputeResources</span></span></td><td><span data-ttu-id="aa4d3-154">hello beräkningsresurser är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-154">hello compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="aa4d3-155">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="aa4d3-155">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="aa4d3-156">Alla instanser av hello aktivitet är upptaget med att köra andra sektorer.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-156">All hello activity instances are busy running other slices.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="aa4d3-157">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="aa4d3-157">ActivityResume</span></span></td><td><span data-ttu-id="aa4d3-158">hello aktiviteten har pausats och kan inte köras hello segment förrän hello aktivitet återupptas.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-158">hello activity is paused and can't run hello slices until hello activity is resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="aa4d3-159">Försök igen</span><span class="sxs-lookup"><span data-stu-id="aa4d3-159">Retry</span></span></td><td><span data-ttu-id="aa4d3-160">Aktivitetskörningen försöks.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-160">Activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="aa4d3-161">Validering</span><span class="sxs-lookup"><span data-stu-id="aa4d3-161">Validation</span></span></td><td><span data-ttu-id="aa4d3-162">Verifieringen har inte startat ännu.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-162">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="aa4d3-163">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="aa4d3-163">ValidationRetry</span></span></td><td><span data-ttu-id="aa4d3-164">Verifieringen är väntar toobe igen.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-164">Validation is waiting toobe retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="aa4d3-165">InProgress</span><span class="sxs-lookup"><span data-stu-id="aa4d3-165">InProgress</span></span></td><td><span data-ttu-id="aa4d3-166">Verifiera</span><span class="sxs-lookup"><span data-stu-id="aa4d3-166">Validating</span></span></td><td><span data-ttu-id="aa4d3-167">Verifiering pågår.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-167">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="aa4d3-168">hello sektorn behandlas.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-168">hello slice is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="aa4d3-169">Det gick inte</span><span class="sxs-lookup"><span data-stu-id="aa4d3-169">Failed</span></span></td><td><span data-ttu-id="aa4d3-170">För lång tid</span><span class="sxs-lookup"><span data-stu-id="aa4d3-170">TimedOut</span></span></td><td><span data-ttu-id="aa4d3-171">hello aktivitetskörningen tog längre tid än vad som tillåts av hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-171">hello activity execution took longer than what is allowed by hello activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="aa4d3-172">Avbrutna</span><span class="sxs-lookup"><span data-stu-id="aa4d3-172">Canceled</span></span></td><td><span data-ttu-id="aa4d3-173">hello sektorn avbröts av en användare.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-173">hello slice was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="aa4d3-174">Validering</span><span class="sxs-lookup"><span data-stu-id="aa4d3-174">Validation</span></span></td><td><span data-ttu-id="aa4d3-175">Verifieringen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-175">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="aa4d3-176">Det gick inte att toobe genereras och/eller verifiera hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-176">hello slice failed toobe generated and/or validated.</span></span></td>
</tr>
<td><span data-ttu-id="aa4d3-177">Redo</span><span class="sxs-lookup"><span data-stu-id="aa4d3-177">Ready</span></span></td><td>-</td><td><span data-ttu-id="aa4d3-178">hello sektorn är klar för användning.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-178">hello slice is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="aa4d3-179">Hoppades över</span><span class="sxs-lookup"><span data-stu-id="aa4d3-179">Skipped</span></span></td><td><span data-ttu-id="aa4d3-180">Ingen</span><span class="sxs-lookup"><span data-stu-id="aa4d3-180">None</span></span></td><td><span data-ttu-id="aa4d3-181">hello segment bearbetas inte.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-181">hello slice isn't being processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="aa4d3-182">Ingen</span><span class="sxs-lookup"><span data-stu-id="aa4d3-182">None</span></span></td><td>-</td><td><span data-ttu-id="aa4d3-183">En sektor används tooexist med en annan status, men den har återställts.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-183">A slice used tooexist with a different status, but it has been reset.</span></span></td>
</tr>
</table>



<span data-ttu-id="aa4d3-184">Du kan visa hello information om en sektor genom att klicka på en post i segment på hello **nyligen uppdaterat segment** bladet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-184">You can view hello details about a slice by clicking a slice entry on hello **Recently Updated Slices** blade.</span></span>

![Sektorn information](./media/data-factory-monitor-manage-pipelines/slice-details.png)

<span data-ttu-id="aa4d3-186">Om hello segment har körts flera gånger, ser du flera rader i hello **aktiviteten körs** lista.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-186">If hello slice has been executed multiple times, you see multiple rows in hello **Activity runs** list.</span></span> <span data-ttu-id="aa4d3-187">Du kan visa information om en aktivitet som kör genom att klicka på Kör hello post i hello **aktiviteten körs** lista.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-187">You can view details about an activity run by clicking hello run entry in hello **Activity runs** list.</span></span> <span data-ttu-id="aa4d3-188">hello i listan visas alla hello-loggfiler, tillsammans med ett felmeddelande om det finns en.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-188">hello list shows all hello log files, along with an error message if there is one.</span></span> <span data-ttu-id="aa4d3-189">Den här funktionen är användbart tooview och felsökningsloggar loggar utan tooleave din data factory.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-189">This feature is useful tooview and debug logs without having tooleave your data factory.</span></span>

![Aktivitetskörningsinformation](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

<span data-ttu-id="aa4d3-191">Om det inte finns i hello hello sektorn **klar** tillstånd, som du kan se hello överordnade sektorer som inte är klar och blockerar hello aktuellt segment från att köras i hello **sektorer uppströms som inte är redo** lista.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-191">If hello slice isn't in hello **Ready** state, you can see hello upstream slices that aren't ready and are blocking hello current slice from executing in hello **Upstream slices that are not ready** list.</span></span> <span data-ttu-id="aa4d3-192">Den här funktionen är användbart när din sektorn är i **väntar på** tillstånd och du vill att toounderstand hello uppströmsberoendena som hello sektorn väntar.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-192">This feature is useful when your slice is in **Waiting** state and you want toounderstand hello upstream dependencies that hello slice is waiting on.</span></span>

![Sektorer uppströms som inte är klara](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a><span data-ttu-id="aa4d3-194">DataSet tillståndsdiagram</span><span class="sxs-lookup"><span data-stu-id="aa4d3-194">Dataset state diagram</span></span>
<span data-ttu-id="aa4d3-195">När du distribuerar en datafabrik och hello pipelines har en ogiltig aktiv period, sektorer övergång från ett tillstånd tooanother hello dataset.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-195">After you deploy a data factory and hello pipelines have a valid active period, hello dataset slices transition from one state tooanother.</span></span> <span data-ttu-id="aa4d3-196">För närvarande följande hello sektorn status hello följande tillståndsdiagram:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-196">Currently, hello slice status follows hello following state diagram:</span></span>

![Tillståndsdiagram](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

<span data-ttu-id="aa4d3-198">hello dataset tillstånd övergången flödet i data factory är hello följande: väntar -> förlopp/i-pågående (verifierar) -> klar/misslyckades.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-198">hello dataset state transition flow in data factory is hello following: Waiting -> In-Progress/In-Progress (Validating) -> Ready/Failed.</span></span>

<span data-ttu-id="aa4d3-199">hello sektorn startar i en **väntar på** tillstånd, väntar på villkor toobe uppfyllda innan den körs.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-199">hello slice starts in a **Waiting** state, waiting for preconditions toobe met before it executes.</span></span> <span data-ttu-id="aa4d3-200">Körningen av hello aktiviteten startar sedan och hello sektorn försätts i ett **pågående** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-200">Then, hello activity starts executing, and hello slice goes into an **In-Progress** state.</span></span> <span data-ttu-id="aa4d3-201">Hej aktivitetskörningen kan lyckas eller misslyckas.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-201">hello activity execution might succeed or fail.</span></span> <span data-ttu-id="aa4d3-202">hello segment har markerats som **klar** eller **misslyckades**, baserat på hello resultatet av hello körning.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-202">hello slice is marked as **Ready** or **Failed**, based on hello result of hello execution.</span></span>

<span data-ttu-id="aa4d3-203">Du kan återställa hello sektorn toogo tillbaka från hello **klar** eller **misslyckades** tillstånd toohello **väntar på** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-203">You can reset hello slice toogo back from hello **Ready** or **Failed** state toohello **Waiting** state.</span></span> <span data-ttu-id="aa4d3-204">Du kan också markera hello sektorn tillstånd för**hoppa över**, vilket förhindrar hello aktivitet från körs och inte behandlar hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-204">You can also mark hello slice state too**Skip**, which prevents hello activity from executing and not processing hello slice.</span></span>

## <a name="pause-and-resume-pipelines"></a><span data-ttu-id="aa4d3-205">Pausa och återuppta pipelines</span><span class="sxs-lookup"><span data-stu-id="aa4d3-205">Pause and resume pipelines</span></span>
<span data-ttu-id="aa4d3-206">Du kan hantera dina pipelines med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-206">You can manage your pipelines by using Azure PowerShell.</span></span> <span data-ttu-id="aa4d3-207">Du kan till exempel pausa och återuppta pipelines genom att köra Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-207">For example, you can pause and resume pipelines by running Azure PowerShell cmdlets.</span></span> 

> [!NOTE] 
> <span data-ttu-id="aa4d3-208">hello diagramvyn stöder inte pausa och återuppta pipelines.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-208">hello diagram view does not support pausing and resuming pipelines.</span></span> <span data-ttu-id="aa4d3-209">Om du vill toouse ett användargränssnitt, Använd hello övervaka och hantera program.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-209">If you want toouse an user interface, use hello monitoring and managing application.</span></span> <span data-ttu-id="aa4d3-210">Mer information om hur du använder programmet hello finns [övervaka och hantera Data Factory pipelines genom att använda appen för övervakning och hantering av hello](data-factory-monitor-manage-app.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-210">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

<span data-ttu-id="aa4d3-211">Du kan Pausa/Pausa pipelines med hjälp av hello **pausa AzureRmDataFactoryPipeline** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-211">You can pause/suspend pipelines by using hello **Suspend-AzureRmDataFactoryPipeline** PowerShell cmdlet.</span></span> <span data-ttu-id="aa4d3-212">Denna cmdlet är användbar när du inte vill toorun din pipelines förrän problemet har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-212">This cmdlet is useful when you don’t want toorun your pipelines until an issue is fixed.</span></span> 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="aa4d3-213">Exempel:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-213">For example:</span></span>

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

<span data-ttu-id="aa4d3-214">Efter hello problemet har korrigerats med hello pipeline, kan du återuppta hello avbruten pipeline genom att köra följande PowerShell-kommando hello:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-214">After hello issue has been fixed with hello pipeline, you can resume hello suspended pipeline by running hello following PowerShell command:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="aa4d3-215">Exempel:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-215">For example:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a><span data-ttu-id="aa4d3-216">Felsöka pipelines</span><span class="sxs-lookup"><span data-stu-id="aa4d3-216">Debug pipelines</span></span>
<span data-ttu-id="aa4d3-217">Azure Data Factory ger omfattande funktioner för du toodebug och felsöka pipelines med hjälp av hello Azure-portalen och Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-217">Azure Data Factory provides rich capabilities for you toodebug and troubleshoot pipelines by using hello Azure portal and Azure PowerShell.</span></span>

> <span data-ttu-id="aa4d3-218">[! Observera} är det mycket enklare tootroubleshot fel med hello övervakning & Management-appen.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-218">[!NOTE} It is much easier tootroubleshot errors using hello Monitoring & Management App.</span></span> <span data-ttu-id="aa4d3-219">Mer information om hur du använder programmet hello finns [övervaka och hantera Data Factory pipelines genom att använda appen för övervakning och hantering av hello](data-factory-monitor-manage-app.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-219">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

### <a name="find-errors-in-a-pipeline"></a><span data-ttu-id="aa4d3-220">Sök efter fel i en pipeline</span><span class="sxs-lookup"><span data-stu-id="aa4d3-220">Find errors in a pipeline</span></span>
<span data-ttu-id="aa4d3-221">Om hello aktiviteten körs inte i en pipeline, är hello dataset som produceras av hello pipeline i ett feltillstånd på grund av hello-fel.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-221">If hello activity run fails in a pipeline, hello dataset that is produced by hello pipeline is in an error state because of hello failure.</span></span> <span data-ttu-id="aa4d3-222">Du kan felsöka och felsöka i Azure Data Factory med hjälp av följande metoder hello.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-222">You can debug and troubleshoot errors in Azure Data Factory by using hello following methods.</span></span>

#### <a name="use-hello-azure-portal-toodebug-an-error"></a><span data-ttu-id="aa4d3-223">Använd hello Azure portal toodebug ett fel</span><span class="sxs-lookup"><span data-stu-id="aa4d3-223">Use hello Azure portal toodebug an error</span></span>
1. <span data-ttu-id="aa4d3-224">På hello **tabell** bladet, klickar du på hello problemet sektor som har hello **Status** ställa in också**misslyckades**.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-224">On hello **Table** blade, click hello problem slice that has hello **Status** set too**Failed**.</span></span>

   ![Tabell bladet med problem](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. <span data-ttu-id="aa4d3-226">På hello **datasektorn** bladet, klickar du på hello-aktivitet som körs som misslyckades.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-226">On hello **Data slice** blade, click hello activity run that failed.</span></span>

   ![Datasektorn med ett fel](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. <span data-ttu-id="aa4d3-228">På hello **aktivitet köras information** bladet som du kan hämta hello-filer som är associerade med hello HDInsight bearbetning.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-228">On hello **Activity run details** blade, you can download hello files that are associated with hello HDInsight processing.</span></span> <span data-ttu-id="aa4d3-229">Klicka på **hämta** för Status/stderr toodownload hello felloggen som innehåller information om hello felet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-229">Click **Download** for Status/stderr toodownload hello error log file that contains details about hello error.</span></span>

   ![Aktiviteten kör informationsbladet med fel](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-toodebug-an-error"></a><span data-ttu-id="aa4d3-231">Använd PowerShell toodebug ett fel</span><span class="sxs-lookup"><span data-stu-id="aa4d3-231">Use PowerShell toodebug an error</span></span>
1. <span data-ttu-id="aa4d3-232">Starta **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-232">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="aa4d3-233">Kör hello **Get-AzureRmDataFactorySlice** kommandot toosee hello segment och deras status.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-233">Run hello **Get-AzureRmDataFactorySlice** command toosee hello slices and their statuses.</span></span> <span data-ttu-id="aa4d3-234">Du bör se ett segment med hello status **misslyckades**.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-234">You should see a slice with hello status of **Failed**.</span></span>        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   <span data-ttu-id="aa4d3-235">Exempel:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-235">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   <span data-ttu-id="aa4d3-236">Ersätt **StartDateTime** med starttiden för din pipeline.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-236">Replace **StartDateTime** with start time of your pipeline.</span></span> 
3. <span data-ttu-id="aa4d3-237">Kör nu hello **Get-AzureRmDataFactoryRun** cmdlet tooget information om hello aktiviteter körs för hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-237">Now, run hello **Get-AzureRmDataFactoryRun** cmdlet tooget details about hello activity run for hello slice.</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    <span data-ttu-id="aa4d3-238">Exempel:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-238">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    <span data-ttu-id="aa4d3-239">hello-värdet för StartDateTime är hello starttiden för hello fel/problem sektor som du antecknade från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-239">hello value of StartDateTime is hello start time for hello error/problem slice that you noted from hello previous step.</span></span> <span data-ttu-id="aa4d3-240">hello tid ska omges av dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-240">hello date-time should be enclosed in double quotes.</span></span>
4. <span data-ttu-id="aa4d3-241">Du bör se utdata med information om felet i hello som är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-241">You should see output with details about hello error that is similar toohello following:</span></span>

    ```   
    Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
    ResourceGroupName       : ADF
    DataFactoryName         : LogProcessingFactory3
    DatasetName               : EnrichedGameEventsTable
    ProcessingStartTime     : 10/10/2014 3:04:52 AM
    ProcessingEndTime       : 10/10/2014 3:06:49 AM
    PercentComplete         : 0
    DataSliceStart          : 5/5/2014 12:00:00 AM
    DataSliceEnd            : 5/6/2014 12:00:00 AM
    Status                  : FailedExecution
    Timestamp               : 10/10/2014 3:04:52 AM
    RetryAttempt            : 0
    Properties              : {}
    ErrorMessage            : Pig script failed with exit code '5'. See wasb://        adfjobs@spestore.blob.core.windows.net/PigQuery
                                    Jobs/841b77c9-d56c-48d1-99a3-
                8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                more details.
    ActivityName            : PigEnrichLogs
    PipelineName            : EnrichGameLogsPipeline
    Type                    :
    ```
5. <span data-ttu-id="aa4d3-242">Du kan köra hello **spara AzureRmDataFactoryLog** med hello ID-värde som du ser från hello-utdata och hämta hello loggfiler med hjälp av hello **- DownloadLogsoption** för hello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-242">You can run hello **Save-AzureRmDataFactoryLog** cmdlet with hello Id value that you see from hello output, and download hello log files by using hello **-DownloadLogsoption** for hello cmdlet.</span></span>

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a><span data-ttu-id="aa4d3-243">Kör fel i en pipeline</span><span class="sxs-lookup"><span data-stu-id="aa4d3-243">Rerun failures in a pipeline</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aa4d3-244">Det är enklare tootroubleshoot fel och kör misslyckade segment med hjälp av hello övervakning & Management-appen.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-244">It's easier tootroubleshoot errors and rerun failed slices by using hello Monitoring & Management App.</span></span> <span data-ttu-id="aa4d3-245">Mer information om hur du använder programmet hello finns [övervaka och hantera Data Factory pipelines genom att använda appen för övervakning och hantering av hello](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="aa4d3-245">For details about using hello application, see [monitor and manage Data Factory pipelines by using hello Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 

### <a name="use-hello-azure-portal"></a><span data-ttu-id="aa4d3-246">Använd hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="aa4d3-246">Use hello Azure portal</span></span>
<span data-ttu-id="aa4d3-247">När du felsöker och felsöka fel i en pipeline, kan du köra fel genom att gå toohello fel segment och klicka på hello **kör** hello kommandofältet-knappen.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-247">After you troubleshoot and debug failures in a pipeline, you can rerun failures by navigating toohello error slice and clicking hello **Run** button on hello command bar.</span></span>

![Kör en misslyckad sektor](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

<span data-ttu-id="aa4d3-249">I fallet hello sektorn valideringen misslyckades på grund av en princip-fel (till exempel om data inte är tillgängligt), kan du åtgärda hello fel och validera igen genom att klicka på hello **verifiera** hello kommandofältet-knappen.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-249">In case hello slice has failed validation because of a policy failure (for example, if data isn't available), you can fix hello failure and validate again by clicking hello **Validate** button on hello command bar.</span></span>

![Åtgärda felen och validera](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a><span data-ttu-id="aa4d3-251">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa4d3-251">Use Azure PowerShell</span></span>
<span data-ttu-id="aa4d3-252">Du kan köra fel med hjälp av hello **Set AzureRmDataFactorySliceStatus** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-252">You can rerun failures by using hello **Set-AzureRmDataFactorySliceStatus** cmdlet.</span></span> <span data-ttu-id="aa4d3-253">Se hello [Set AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) avsnittet annan information om hello-cmdlet och syntax.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-253">See hello [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) topic for syntax and other details about hello cmdlet.</span></span>

<span data-ttu-id="aa4d3-254">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-254">**Example:**</span></span>

<span data-ttu-id="aa4d3-255">hello följande exempel anger hello status för alla segment för hello tabell 'DAWikiAggregatedData' too'Waiting' i hello Azure data factory 'WikiADF'.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-255">hello following example sets hello status of all slices for hello table 'DAWikiAggregatedData' too'Waiting' in hello Azure data factory 'WikiADF'.</span></span>

<span data-ttu-id="aa4d3-256">hello 'Uppdateringstyp' anges too'UpstreamInPipeline ', vilket innebär att statusen för varje segment för hello tabellen och alla hello beroende (överordnad) tabeller är inställda too'Waiting'.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-256">hello 'UpdateType' is set too'UpstreamInPipeline', which means that statuses of each slice for hello table and all hello dependent (upstream) tables are set too'Waiting'.</span></span> <span data-ttu-id="aa4d3-257">hello är andra möjliga värde för den här parametern 'Person'.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-257">hello other possible value for this parameter is 'Individual'.</span></span>

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```

## <a name="create-alerts"></a><span data-ttu-id="aa4d3-258">Skapa aviseringar</span><span class="sxs-lookup"><span data-stu-id="aa4d3-258">Create alerts</span></span>
<span data-ttu-id="aa4d3-259">Användaren Azure loggar händelser när en Azure-resurs (till exempel data factory) skapas, uppdateras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-259">Azure logs user events when an Azure resource (for example, a data factory) is created, updated, or deleted.</span></span> <span data-ttu-id="aa4d3-260">Du kan skapa aviseringar på dessa händelser.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-260">You can create alerts on these events.</span></span> <span data-ttu-id="aa4d3-261">Du kan använda Data Factory toocapture olika mått och skapa aviseringar på mått.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-261">You can use Data Factory toocapture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="aa4d3-262">Vi rekommenderar att du använder händelser för övervakning i realtid och använda mått i historiksyfte.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-262">We recommend that you use events for real-time monitoring and use metrics for historical purposes.</span></span>

### <a name="alerts-on-events"></a><span data-ttu-id="aa4d3-263">Aviseringar om händelser</span><span class="sxs-lookup"><span data-stu-id="aa4d3-263">Alerts on events</span></span>
<span data-ttu-id="aa4d3-264">Azure händelser ger användbar insikter om vad som händer i Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-264">Azure events provide useful insights into what is happening in your Azure resources.</span></span> <span data-ttu-id="aa4d3-265">När du använder Azure Data Factory händelser genereras när:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-265">When you're using Azure Data Factory, events are generated when:</span></span>

* <span data-ttu-id="aa4d3-266">En datafabrik har skapats, uppdateras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-266">A data factory is created, updated, or deleted.</span></span>
* <span data-ttu-id="aa4d3-267">Databearbetning (som ”kör”) har startats eller slutförts.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-267">Data processing (as "runs") has started or completed.</span></span>
* <span data-ttu-id="aa4d3-268">Ett HDInsight-kluster på begäran skapas eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-268">An on-demand HDInsight cluster is created or removed.</span></span>

<span data-ttu-id="aa4d3-269">Du kan skapa varningar på dessa användarhändelser och konfigurera dem toosend e-postaviseringar toohello administratör och coadministrators hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-269">You can create alerts on these user events and configure them toosend email notifications toohello administrator and coadministrators of hello subscription.</span></span> <span data-ttu-id="aa4d3-270">Dessutom kan du ange ytterligare e-postadresserna för användare som behöver tooreceive e-postaviseringar när hello villkor är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-270">In addition, you can specify additional email addresses of users who need tooreceive email notifications when hello conditions are met.</span></span> <span data-ttu-id="aa4d3-271">Den här funktionen är användbart när du vill meddelas när fel tooget och inte vill toocontinuously övervaka din data factory.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-271">This feature is useful when you want tooget notified on failures and don’t want toocontinuously monitor your data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="aa4d3-272">För närvarande visar hello portal inte aviseringar om händelser.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-272">Currently, hello portal doesn't show alerts on events.</span></span> <span data-ttu-id="aa4d3-273">Använd hello [övervakning och hantering av](data-factory-monitor-manage-app.md) toosee alla aviseringar.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-273">Use hello [Monitoring and Management app](data-factory-monitor-manage-app.md) toosee all alerts.</span></span>


#### <a name="specify-an-alert-definition"></a><span data-ttu-id="aa4d3-274">Ange en definition av varning</span><span class="sxs-lookup"><span data-stu-id="aa4d3-274">Specify an alert definition</span></span>
<span data-ttu-id="aa4d3-275">toospecify en varningsdefinitionen du skapa en JSON-fil som beskriver hello-åtgärder som du vill att ett meddelande om toobe.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-275">toospecify an alert definition, you create a JSON file that describes hello operations that you want toobe alerted on.</span></span> <span data-ttu-id="aa4d3-276">I följande exempel hello, skickar hello avisering ett e-postmeddelande för hello RunFinished igen.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-276">In hello following example, hello alert sends an email notification for hello RunFinished operation.</span></span> <span data-ttu-id="aa4d3-277">toobe specifika, ett e-postmeddelande skickas när en körning i hello data factory har slutförts och inte det gick att köra hello (Status = FailedExecution).</span><span class="sxs-lookup"><span data-stu-id="aa4d3-277">toobe specific, an email notification is sent when a run in hello data factory has completed and hello run has failed (Status = FailedExecution).</span></span>

```JSON
{
    "contentVersion": "1.0.0.0",
     "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters": {},
    "resources":
    [
        {
            "name": "ADFAlertsSlice",
            "type": "microsoft.insights/alertrules",
            "apiVersion": "2014-04-01",
            "location": "East US",
            "properties":
            {
                "name": "ADFAlertsSlice",
                "description": "One or more of hello data slices for hello Azure Data Factory has failed processing.",
                "isEnabled": true,
                "condition":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                    "dataSource":
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                        "operationName": "RunFinished",
                        "status": "Failed",
                        "subStatus": "FailedExecution"   
                    }
                },
                "action":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails": [ "<your alias>@contoso.com" ]
                }
            }
        }
    ]
}
```

<span data-ttu-id="aa4d3-278">Du kan ta bort **subStatus** från hello JSON-definitionen om du inte vill toobe ett meddelande om ett specifikt fel.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-278">You can remove **subStatus** from hello JSON definition if you don’t want toobe alerted on a specific failure.</span></span>

<span data-ttu-id="aa4d3-279">Det här exemplet konfigurerar hello varning för alla datafabriker i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-279">This example sets up hello alert for all data factories in your subscription.</span></span> <span data-ttu-id="aa4d3-280">Om du vill hello avisering toobe har ställts in för en viss datafabrik, kan du ange data factory **resourceUri** i hello **dataSource**:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-280">If you want hello alert toobe set up for a particular data factory, you can specify data factory **resourceUri** in hello **dataSource**:</span></span>

```JSON
"resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"
```

<span data-ttu-id="aa4d3-281">hello innehåller följande tabell hello lista över tillgängliga åtgärder och status (och underordnad status).</span><span class="sxs-lookup"><span data-stu-id="aa4d3-281">hello following table provides hello list of available operations and statuses (and substatuses).</span></span>

| <span data-ttu-id="aa4d3-282">Åtgärdsnamn</span><span class="sxs-lookup"><span data-stu-id="aa4d3-282">Operation name</span></span> | <span data-ttu-id="aa4d3-283">Status</span><span class="sxs-lookup"><span data-stu-id="aa4d3-283">Status</span></span> | <span data-ttu-id="aa4d3-284">Substatus</span><span class="sxs-lookup"><span data-stu-id="aa4d3-284">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="aa4d3-285">RunStarted</span><span class="sxs-lookup"><span data-stu-id="aa4d3-285">RunStarted</span></span> |<span data-ttu-id="aa4d3-286">Igång</span><span class="sxs-lookup"><span data-stu-id="aa4d3-286">Started</span></span> |<span data-ttu-id="aa4d3-287">Startar</span><span class="sxs-lookup"><span data-stu-id="aa4d3-287">Starting</span></span> |
| <span data-ttu-id="aa4d3-288">RunFinished</span><span class="sxs-lookup"><span data-stu-id="aa4d3-288">RunFinished</span></span> |<span data-ttu-id="aa4d3-289">Misslyckades / lyckades</span><span class="sxs-lookup"><span data-stu-id="aa4d3-289">Failed / Succeeded</span></span> |<span data-ttu-id="aa4d3-290">FailedResourceAllocation</span><span class="sxs-lookup"><span data-stu-id="aa4d3-290">FailedResourceAllocation</span></span><br/><br/><span data-ttu-id="aa4d3-291">Lyckades</span><span class="sxs-lookup"><span data-stu-id="aa4d3-291">Succeeded</span></span><br/><br/><span data-ttu-id="aa4d3-292">FailedExecution</span><span class="sxs-lookup"><span data-stu-id="aa4d3-292">FailedExecution</span></span><br/><br/><span data-ttu-id="aa4d3-293">För lång tid</span><span class="sxs-lookup"><span data-stu-id="aa4d3-293">TimedOut</span></span><br/><br/><span data-ttu-id="aa4d3-294">< avbröts</span><span class="sxs-lookup"><span data-stu-id="aa4d3-294"><Canceled</span></span><br/><br/><span data-ttu-id="aa4d3-295">FailedValidation</span><span class="sxs-lookup"><span data-stu-id="aa4d3-295">FailedValidation</span></span><br/><br/><span data-ttu-id="aa4d3-296">Avbrutna</span><span class="sxs-lookup"><span data-stu-id="aa4d3-296">Abandoned</span></span> |
| <span data-ttu-id="aa4d3-297">OnDemandClusterCreateStarted</span><span class="sxs-lookup"><span data-stu-id="aa4d3-297">OnDemandClusterCreateStarted</span></span> |<span data-ttu-id="aa4d3-298">Igång</span><span class="sxs-lookup"><span data-stu-id="aa4d3-298">Started</span></span> | |
| <span data-ttu-id="aa4d3-299">OnDemandClusterCreateSuccessful</span><span class="sxs-lookup"><span data-stu-id="aa4d3-299">OnDemandClusterCreateSuccessful</span></span> |<span data-ttu-id="aa4d3-300">Lyckades</span><span class="sxs-lookup"><span data-stu-id="aa4d3-300">Succeeded</span></span> | |
| <span data-ttu-id="aa4d3-301">OnDemandClusterDeleted</span><span class="sxs-lookup"><span data-stu-id="aa4d3-301">OnDemandClusterDeleted</span></span> |<span data-ttu-id="aa4d3-302">Lyckades</span><span class="sxs-lookup"><span data-stu-id="aa4d3-302">Succeeded</span></span> | |

<span data-ttu-id="aa4d3-303">Se [skapa Varningsregeln](https://msdn.microsoft.com/library/azure/dn510366.aspx) för ytterligare information om hello JSON-element som används i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-303">See [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) for details about hello JSON elements that are used in hello example.</span></span>

#### <a name="deploy-hello-alert"></a><span data-ttu-id="aa4d3-304">Distribuera hello avisering</span><span class="sxs-lookup"><span data-stu-id="aa4d3-304">Deploy hello alert</span></span>
<span data-ttu-id="aa4d3-305">toodeploy hello avisering, Använd hello Azure PowerShell-cmdleten **ny AzureRmResourceGroupDeployment**som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-305">toodeploy hello alert, use hello Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in hello following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  
```

<span data-ttu-id="aa4d3-306">När hello resurs distribution har slutförts visas hello följande meddelanden:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-306">After hello resource group deployment has finished successfully, you see hello following messages:</span></span>

```
VERBOSE: 7:00:48 PM - Template is valid.
WARNING: 7:00:48 PM - hello StorageAccountName parameter is no longer used and will be removed in a future release.
Please update scripts tooremove this parameter.
VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded

DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

> [!NOTE]
> <span data-ttu-id="aa4d3-307">Du kan använda hello [Skapa regel för varning](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API toocreate en aviseringsregel.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-307">You can use hello [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API toocreate an alert rule.</span></span> <span data-ttu-id="aa4d3-308">hello JSON-nyttolast är liknande toohello JSON-exempel.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-308">hello JSON payload is similar toohello JSON example.</span></span>  


#### <a name="retrieve-hello-list-of-azure-resource-group-deployments"></a><span data-ttu-id="aa4d3-309">Hämta hello lista över distributionen av Azure resursgrupper</span><span class="sxs-lookup"><span data-stu-id="aa4d3-309">Retrieve hello list of Azure resource group deployments</span></span>
<span data-ttu-id="aa4d3-310">tooretrieve hello lista över distribuerade distributionen av Azure resursgrupper använder hello cmdlet **Get-AzureRmResourceGroupDeployment**som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-310">tooretrieve hello list of deployed Azure resource group deployments, use hello cmdlet **Get-AzureRmResourceGroupDeployment**, as shown in hello following example:</span></span>

```powershell
Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
```

```
DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

#### <a name="troubleshoot-user-events"></a><span data-ttu-id="aa4d3-311">Felsöka användarhändelser</span><span class="sxs-lookup"><span data-stu-id="aa4d3-311">Troubleshoot user events</span></span>
1. <span data-ttu-id="aa4d3-312">Du kan se alla hello-händelser som genereras när du klickar på hello **mått och åtgärder** panelen.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-312">You can see all hello events that are generated after clicking hello **Metrics and operations** tile.</span></span>

    ![Panelen mått och åtgärder](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)
2. <span data-ttu-id="aa4d3-314">Klicka på hello **händelser** panelen toosee hello händelser.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-314">Click hello **Events** tile toosee hello events.</span></span>

    ![Panelen händelser](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. <span data-ttu-id="aa4d3-316">På hello **händelser** bladet hittar du information om händelser, filtreras händelser och så vidare.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-316">On hello **Events** blade, you can see details about events, filtered events, and so on.</span></span>

    ![Händelser bladet](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. <span data-ttu-id="aa4d3-318">Klicka på en **åtgärden** i hello operations lista som orsakar ett fel.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-318">Click an **Operation** in hello operations list that causes an error.</span></span>

    ![Välj en åtgärd](./media/data-factory-monitor-manage-pipelines/select-operation.png)
5. <span data-ttu-id="aa4d3-320">Klicka på en **fel** toosee händelseinformationen om hello felet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-320">Click an **Error** event toosee details about hello error.</span></span>

    ![Händelsefel](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)

<span data-ttu-id="aa4d3-322">Se [Azure Insight cmdlets](https://msdn.microsoft.com/library/mt282452.aspx) för PowerShell-cmdlets som du kan använda tooadd, hämta eller ta bort aviseringar.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-322">See [Azure Insight cmdlets](https://msdn.microsoft.com/library/mt282452.aspx) for PowerShell cmdlets that you can use tooadd, get, or remove alerts.</span></span> <span data-ttu-id="aa4d3-323">Här följer några exempel på användning av hello **Get-AlertRule** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-323">Here are a few examples of using hello **Get-AlertRule** cmdlet:</span></span>

```powershell
get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
```

```
Properties :
Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
Condition   :
DataSource :
EventName             :
Category              :
Level                 :
OperationName         : RunFinished
ResourceGroupName     :
ResourceProviderName  :
ResourceId            :
Status                : Failed
SubStatus             : FailedExecution
Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
Condition      :
Description : One or more of hello data slices for hello Azure Data Factory has failed processing.
Status      : Enabled
Name:       : ADFAlertsSlice
Tags       :
$type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
Location   : West US
Name       : ADFAlertsSlice
```

```powershell
Get-AlertRule -res $resourceGroup
```
```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0

Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
Location   : West US
Name       : FailedExecutionRunsWest3
```

```powershell
Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
```

```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0
```

<span data-ttu-id="aa4d3-324">Kör hello följande get-help kommandon toosee information och exempel för hello Get-AlertRule cmdlet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-324">Run hello following get-help commands toosee details and examples for hello Get-AlertRule cmdlet.</span></span>

```powershell
get-help Get-AlertRule -detailed
```

```powershell
get-help Get-AlertRule -examples
```


<span data-ttu-id="aa4d3-325">Om du ser hello varningsgenereringen händelser på hello portalbladet men du ta emot inte e-postaviseringar, kontrollera om tooreceive e-post från externa avsändare anges hello e-postadress som har angetts.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-325">If you see hello alert generation events on hello portal blade but you don't receive email notifications, check whether hello email address that is specified is set tooreceive emails from external senders.</span></span> <span data-ttu-id="aa4d3-326">hello aviseringsmeddelanden kanske har blockerats av din e-postinställningar.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-326">hello alert emails might have been blocked by your email settings.</span></span>

### <a name="alerts-on-metrics"></a><span data-ttu-id="aa4d3-327">Aviseringar om mått</span><span class="sxs-lookup"><span data-stu-id="aa4d3-327">Alerts on metrics</span></span>
<span data-ttu-id="aa4d3-328">I Data Factory du samla in olika mått och skapa aviseringar på mått.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-328">In Data Factory, you can capture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="aa4d3-329">Du kan övervaka och skapa aviseringar på följande mätvärden för hello segment i din data factory hello:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-329">You can monitor and create alerts on hello following metrics for hello slices in your data factory:</span></span>

* <span data-ttu-id="aa4d3-330">**Misslyckade körs**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-330">**Failed Runs**</span></span>
* <span data-ttu-id="aa4d3-331">**Lyckad körs**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-331">**Successful Runs**</span></span>

<span data-ttu-id="aa4d3-332">De här måtten är användbara och hjälpa dig en översikt över övergripande misslyckade och lyckade körs i hello data factory tooget.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-332">These metrics are useful and help you tooget an overview of overall failed and successful runs in hello data factory.</span></span> <span data-ttu-id="aa4d3-333">Mått är orsakat varje gång det är ett segment kör.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-333">Metrics are emitted every time there is a slice run.</span></span> <span data-ttu-id="aa4d3-334">De här måtten aggregeras hello början av hello timme och pushas tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-334">At hello beginning of hello hour, these metrics are aggregated and pushed tooyour storage account.</span></span> <span data-ttu-id="aa4d3-335">tooenable statistik, och skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-335">tooenable metrics, set up a storage account.</span></span>

#### <a name="enable-metrics"></a><span data-ttu-id="aa4d3-336">Aktivera mätvärden</span><span class="sxs-lookup"><span data-stu-id="aa4d3-336">Enable metrics</span></span>
<span data-ttu-id="aa4d3-337">tooenable statistik, och klicka på hello följande från hello **Data Factory** bladet:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-337">tooenable metrics, click hello following from hello **Data Factory** blade:</span></span>

<span data-ttu-id="aa4d3-338">**Övervaka** > **mått** > **diagnostikinställningar** > **diagnostik**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-338">**Monitoring** > **Metric** > **Diagnostic settings** > **Diagnostics**</span></span>

![Diagnostik-länk](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

<span data-ttu-id="aa4d3-340">På hello **diagnostik** bladet, klickar du på **på**, Välj hello lagringskonto och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-340">On hello **Diagnostics** blade, click **On**, select hello storage account, and click **Save**.</span></span>

![Diagnostik-bladet](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

<span data-ttu-id="aa4d3-342">Det kan ta upp tooone timme för hello mått toobe synliga på hello **övervakning** bladet eftersom mått aggregering sker varje timme.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-342">It might take up tooone hour for hello metrics toobe visible on hello **Monitoring** blade because metrics aggregation happens hourly.</span></span>

### <a name="set-up-an-alert-on-metrics"></a><span data-ttu-id="aa4d3-343">Skapa en avisering om mått</span><span class="sxs-lookup"><span data-stu-id="aa4d3-343">Set up an alert on metrics</span></span>
<span data-ttu-id="aa4d3-344">Klicka på hello **Data Factory mått** panelen:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-344">Click hello **Data Factory metrics** tile:</span></span>

![Data factory mått sida vid sida](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

<span data-ttu-id="aa4d3-346">På hello **mått** bladet, klickar du på **+ Lägg till avisering** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-346">On hello **Metric** blade, click **+ Add alert** on hello toolbar.</span></span>
<span data-ttu-id="aa4d3-347">![Data factory mått bladet > Lägg till avisering](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span><span class="sxs-lookup"><span data-stu-id="aa4d3-347">![Data factory metric blade > Add alert](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span></span>

<span data-ttu-id="aa4d3-348">På hello **lägga till en varningsregel** gör hello följande steg, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-348">On hello **Add an alert rule** page, do hello following steps, and click **OK**.</span></span>

* <span data-ttu-id="aa4d3-349">Ange ett namn för aviseringen hello (exempel: ”misslyckades aviseringen”).</span><span class="sxs-lookup"><span data-stu-id="aa4d3-349">Enter a name for hello alert (example: "failed alert").</span></span>
* <span data-ttu-id="aa4d3-350">Ange en beskrivning för hello varning (exempel: ”skicka ett e-postmeddelande när ett fel uppstår”).</span><span class="sxs-lookup"><span data-stu-id="aa4d3-350">Enter a description for hello alert (example: "send an email when a failure occurs").</span></span>
* <span data-ttu-id="aa4d3-351">Markera ett mått (vs ”kunde inte körs”. ”Lyckades körs”).</span><span class="sxs-lookup"><span data-stu-id="aa4d3-351">Select a metric ("Failed Runs" vs. "Successful Runs").</span></span>
* <span data-ttu-id="aa4d3-352">Ange ett villkor och ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-352">Specify a condition and a threshold value.</span></span>   
* <span data-ttu-id="aa4d3-353">Ange hello tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-353">Specify hello period of time.</span></span>
* <span data-ttu-id="aa4d3-354">Ange om ett e-postmeddelande ska skickas tooowners, deltagare och läsare.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-354">Specify whether an email should be sent tooowners, contributors, and readers.</span></span>

![Data factory mått bladet > Lägg till regel för varning](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

<span data-ttu-id="aa4d3-356">När hello varningsregeln har lagts till, hello blad stängs och du ser hello ny avisering på hello **mått** bladet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-356">After hello alert rule is added successfully, hello blade closes and you see hello new alert on hello **Metric** blade.</span></span>

![Data factory mått bladet > Ny avisering som lagts till](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

<span data-ttu-id="aa4d3-358">Du bör också se hello antal varningar i hello **Varna regler** panelen.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-358">You should also see hello number of alerts in hello **Alert rules** tile.</span></span> <span data-ttu-id="aa4d3-359">Klicka på hello **Varna regler** panelen.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-359">Click hello **Alert rules** tile.</span></span>

![Data factory mått-bladet – Varningsregler](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

<span data-ttu-id="aa4d3-361">På hello **aviseringar regler** bladet visas alla befintliga varningar.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-361">On hello **Alerts rules** blade, you see any existing alerts.</span></span> <span data-ttu-id="aa4d3-362">tooadd en avisering, klickar du på **Lägg till avisering** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-362">tooadd an alert, click **Add alert** on hello toolbar.</span></span>

![Varningsregler bladet](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a><span data-ttu-id="aa4d3-364">Varningsmeddelanden</span><span class="sxs-lookup"><span data-stu-id="aa4d3-364">Alert notifications</span></span>
<span data-ttu-id="aa4d3-365">När hello varningsregeln matchar hello villkor, bör du få ett e-postmeddelande som säger hello aviseringen har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-365">After hello alert rule matches hello condition, you should get an email that says hello alert is activated.</span></span> <span data-ttu-id="aa4d3-366">När hello problemet har lösts och hello varningsvillkor matchar inte längre kan få du ett e-postmeddelande som säger hello varningen har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-366">After hello issue is resolved and hello alert condition doesn’t match anymore, you get an email that says hello alert is resolved.</span></span>

<span data-ttu-id="aa4d3-367">Det här skiljer sig händelser där skickas ett meddelande på alla fel som en aviseringsregel är berättigad till.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-367">This behavior is different than events where a notification is sent on every failure that an alert rule qualifies for.</span></span>

### <a name="deploy-alerts-by-using-powershell"></a><span data-ttu-id="aa4d3-368">Distribuera aviseringar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa4d3-368">Deploy alerts by using PowerShell</span></span>
<span data-ttu-id="aa4d3-369">Du kan distribuera aviseringar för mått hello samma sätt som för händelser.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-369">You can deploy alerts for metrics hello same way that you do for events.</span></span>

<span data-ttu-id="aa4d3-370">**Varningsdefinitionen**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-370">**Alert definition**</span></span>

```JSON
{
    "contentVersion" : "1.0.0.0",
    "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters" : {},
    "resources" : [
    {
            "name" : "FailedRunsGreaterThan5",
            "type" : "microsoft.insights/alertrules",
            "apiVersion" : "2014-04-01",
            "location" : "East US",
            "properties" : {
                "name" : "FailedRunsGreaterThan5",
                "description" : "Failed Runs greater than 5",
                "isEnabled" : true,
                "condition" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                    "dataSource" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                        "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                        "metricName" : "FailedRuns"
                    },
                    "threshold" : 5.0,
                    "windowSize" : "PT3H",
                    "timeAggregation" : "Total"
                },
                "action" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails" : ["abhinav.gpt@live.com"]
                }
            }
        }
    ]
}
```

<span data-ttu-id="aa4d3-371">Ersätt *subscriptionId*, *resourceGroupName*, och *dataFactoryName* i hello exemplet med lämpliga värden.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-371">Replace *subscriptionId*, *resourceGroupName*, and *dataFactoryName* in hello sample with appropriate values.</span></span>

<span data-ttu-id="aa4d3-372">*metricName* stöder för närvarande två värden:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-372">*metricName* currently supports two values:</span></span>

* <span data-ttu-id="aa4d3-373">FailedRuns</span><span class="sxs-lookup"><span data-stu-id="aa4d3-373">FailedRuns</span></span>
* <span data-ttu-id="aa4d3-374">SuccessfulRuns</span><span class="sxs-lookup"><span data-stu-id="aa4d3-374">SuccessfulRuns</span></span>

<span data-ttu-id="aa4d3-375">**Distribuera hello avisering**</span><span class="sxs-lookup"><span data-stu-id="aa4d3-375">**Deploy hello alert**</span></span>

<span data-ttu-id="aa4d3-376">toodeploy hello avisering, Använd hello Azure PowerShell-cmdleten **ny AzureRmResourceGroupDeployment**som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-376">toodeploy hello alert, use hello Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in hello following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json
```

<span data-ttu-id="aa4d3-377">Du bör se följande meddelande efter en lyckad distribution:</span><span class="sxs-lookup"><span data-stu-id="aa4d3-377">You should see following message after a successful deployment:</span></span>

```
VERBOSE: 12:52:47 PM - Template is valid.
VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded


DeploymentName    : FailedRunsGreaterThan5
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 7/27/2015 7:52:56 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           
```

<span data-ttu-id="aa4d3-378">Du kan också använda hello **Lägg till AlertRule** cmdlet toodeploy en aviseringsregel.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-378">You can also use hello **Add-AlertRule** cmdlet toodeploy an alert rule.</span></span> <span data-ttu-id="aa4d3-379">Se hello [Lägg till AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) avsnittet information och exempel.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-379">See hello [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) topic for details and examples.</span></span>  

## <a name="move-a-data-factory-tooa-different-resource-group-or-subscription"></a><span data-ttu-id="aa4d3-380">Flytta en data factory tooa annan resursgrupp eller prenumeration</span><span class="sxs-lookup"><span data-stu-id="aa4d3-380">Move a data factory tooa different resource group or subscription</span></span>
<span data-ttu-id="aa4d3-381">Du kan flytta data factory tooa olika resursgrupper eller en annan prenumeration med hjälp av hello **flytta** kommandot liggande hello startsidan i din data factory-knappen.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-381">You can move a data factory tooa different resource group or a different subscription by using hello **Move** command bar button on hello home page of your data factory.</span></span>

![Flytta data factory](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

<span data-ttu-id="aa4d3-383">Du kan också flytta alla relaterade resurser (till exempel aviseringar som är associerade med hello data factory), tillsammans med hello data factory.</span><span class="sxs-lookup"><span data-stu-id="aa4d3-383">You can also move any related resources (such as alerts that are associated with hello data factory), along with hello data factory.</span></span>

![Flytta dialogrutan resurser](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
