---
title: "Övervaka och hantera pipelines med hjälp av Azure portal och PowerShell | Microsoft Docs"
description: "Lär dig hur du använder Azure-portalen och Azure PowerShell för att övervaka och hantera Azure datafabriker och pipelines som du har skapat."
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
ms.openlocfilehash: 61bb5379cd94dd00814e14420947e7783999ff0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-azure-portal-and-powershell"></a><span data-ttu-id="69418-103">Övervaka och hantera Azure Data Factory pipelines med hjälp av Azure portal och PowerShell</span><span class="sxs-lookup"><span data-stu-id="69418-103">Monitor and manage Azure Data Factory pipelines by using the Azure portal and PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="69418-104">Med hjälp av Azure portal/Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="69418-104">Using Azure portal/Azure PowerShell</span></span>](data-factory-monitor-manage-pipelines.md)
> * [<span data-ttu-id="69418-105">Med hjälp av övervakning och Management-appen</span><span class="sxs-lookup"><span data-stu-id="69418-105">Using Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)


> [!IMPORTANT]
> <span data-ttu-id="69418-106">Övervakning och hantering av programmet ger ett bättre stöd för att övervaka och hantera dina data pipelines och felsöka eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="69418-106">The monitoring & management application provides a better support for monitoring and managing your data pipelines, and troubleshooting any issues.</span></span> <span data-ttu-id="69418-107">Mer information om hur du använder programmet finns [övervaka och hantera Data Factory pipelines med hjälp av övervakning och hantering av appen](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="69418-107">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 


<span data-ttu-id="69418-108">Den här artikeln beskriver hur du övervaka, hantera och felsöka din pipelines med hjälp av Azure portal och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="69418-108">This article describes how to monitor, manage, and debug your pipelines by using Azure portal and PowerShell.</span></span> <span data-ttu-id="69418-109">Artikeln innehåller även information om hur du skapar aviseringar och håll dig informerad om misslyckade.</span><span class="sxs-lookup"><span data-stu-id="69418-109">The article also provides information on how to create alerts and get notified about failures.</span></span>

## <a name="understand-pipelines-and-activity-states"></a><span data-ttu-id="69418-110">Förstå pipelines och aktivitet tillstånd</span><span class="sxs-lookup"><span data-stu-id="69418-110">Understand pipelines and activity states</span></span>
<span data-ttu-id="69418-111">Med hjälp av Azure portal, kan du:</span><span class="sxs-lookup"><span data-stu-id="69418-111">By using the Azure portal, you can:</span></span>

* <span data-ttu-id="69418-112">Visa din data factory som ett diagram.</span><span class="sxs-lookup"><span data-stu-id="69418-112">View your data factory as a diagram.</span></span>
* <span data-ttu-id="69418-113">Visa aktiviteter i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="69418-113">View activities in a pipeline.</span></span>
* <span data-ttu-id="69418-114">Visa inkommande och utgående datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="69418-114">View input and output datasets.</span></span>

<span data-ttu-id="69418-115">Det här avsnittet beskriver också hur en datamängdssektor övergångar från ett tillstånd till ett annat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="69418-115">This section also describes how a dataset slice transitions from one state to another state.</span></span>   

### <a name="navigate-to-your-data-factory"></a><span data-ttu-id="69418-116">Navigera till din data factory</span><span class="sxs-lookup"><span data-stu-id="69418-116">Navigate to your data factory</span></span>
1. <span data-ttu-id="69418-117">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="69418-117">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="69418-118">Klicka på **datafabriker** på menyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="69418-118">Click **Data factories** on the menu on the left.</span></span> <span data-ttu-id="69418-119">Om du inte ser det klickar du på **fler tjänster >**, och klicka sedan på **datafabriker** under den **INTELLIGENCE + analys** kategori.</span><span class="sxs-lookup"><span data-stu-id="69418-119">If you don't see it, click **More services >**, and then click **Data factories** under the **INTELLIGENCE + ANALYTICS** category.</span></span>

   ![Bläddra igenom alla > datafabriker](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. <span data-ttu-id="69418-121">På den **datafabriker** bladet välj datafabriken som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="69418-121">On the **Data factories** blade, select the data factory that you're interested in.</span></span>

    ![Välj data factory](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   <span data-ttu-id="69418-123">Du bör se startsidan för datafabriken.</span><span class="sxs-lookup"><span data-stu-id="69418-123">You should see the home page for the data factory.</span></span>

   ![Data factory-bladet](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a><span data-ttu-id="69418-125">Diagramvy i din data factory</span><span class="sxs-lookup"><span data-stu-id="69418-125">Diagram view of your data factory</span></span>
<span data-ttu-id="69418-126">Den **Diagram** vy av en datafabrik som tillhandahåller en och samma plats att övervaka och hantera data factory och dess tillgångar.</span><span class="sxs-lookup"><span data-stu-id="69418-126">The **Diagram** view of a data factory provides a single pane of glass to monitor and manage the data factory and its assets.</span></span> <span data-ttu-id="69418-127">Se den **Diagram** visa i din data factory, klickar du på **Diagram** på startsidan för data factory.</span><span class="sxs-lookup"><span data-stu-id="69418-127">To see the **Diagram** view of your data factory, click **Diagram** on the home page for the data factory.</span></span>

![Diagramvy](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

<span data-ttu-id="69418-129">Du kan zooma in, Zooma ut, Zooma in för att passa, Zooma till 100%, låsa layouten för diagrammet och placera pipelines och datauppsättningar automatiskt.</span><span class="sxs-lookup"><span data-stu-id="69418-129">You can zoom in, zoom out, zoom to fit, zoom to 100%, lock the layout of the diagram, and automatically position pipelines and datasets.</span></span> <span data-ttu-id="69418-130">Du kan också se informationen om härkomst (det vill säga visa överordnade och underordnade objekt av valda objekt).</span><span class="sxs-lookup"><span data-stu-id="69418-130">You can also see the data lineage information (that is, show upstream and downstream items of selected items).</span></span>

### <a name="activities-inside-a-pipeline"></a><span data-ttu-id="69418-131">Aktiviteter i en pipeline</span><span class="sxs-lookup"><span data-stu-id="69418-131">Activities inside a pipeline</span></span>
1. <span data-ttu-id="69418-132">Högerklicka på pipeline och klicka sedan på **öppna pipeline** att se alla aktiviteter i pipelinen, tillsammans med indata- och datauppsättningar för aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="69418-132">Right-click the pipeline, and then click **Open pipeline** to see all activities in the pipeline, along with input and output datasets for the activities.</span></span> <span data-ttu-id="69418-133">Den här funktionen är användbart när din pipeline innehåller mer än en aktivitet och du vill förstå operativa härkomst av en enkel rörledning.</span><span class="sxs-lookup"><span data-stu-id="69418-133">This feature is useful when your pipeline includes more than one activity and you want to understand the operational lineage of a single pipeline.</span></span>

    ![Menyn Öppna pipeline](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. <span data-ttu-id="69418-135">I följande exempel visas en kopia aktivitet i pipelinen med indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="69418-135">In the following example, you see a copy activity in the pipeline with an input and an output.</span></span> 

    ![Aktiviteter i en pipeline](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. <span data-ttu-id="69418-137">Du kan gå tillbaka till startsidan för datafabriken genom att klicka på **datafabriken** länk i sökvägen i det övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="69418-137">You can navigate back to the home page of the data factory by clicking the **Data factory** link in the breadcrumb at the top-left corner.</span></span>

    ![Gå tillbaka till data factory](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-the-state-of-each-activity-inside-a-pipeline"></a><span data-ttu-id="69418-139">Visa status för varje aktivitet i en pipeline</span><span class="sxs-lookup"><span data-stu-id="69418-139">View the state of each activity inside a pipeline</span></span>
<span data-ttu-id="69418-140">Du kan visa det aktuella tillståndet för en aktivitet genom att visa status för alla datauppsättningar som produceras av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="69418-140">You can view the current state of an activity by viewing the status of any of the datasets that are produced by the activity.</span></span>

<span data-ttu-id="69418-141">Genom att dubbelklicka på den **OutputBlobTable** i den **Diagram**, du kan se alla segment som genereras av en annan aktivitet körs i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="69418-141">By double-clicking the **OutputBlobTable** in the **Diagram**, you can see all the slices that are produced by different activity runs inside a pipeline.</span></span> <span data-ttu-id="69418-142">Du kan se att kopieringsaktiviteten kördes har för de senaste åtta timmarna och producerade sektorer i den **klar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="69418-142">You can see that the copy activity ran successfully for the last eight hours and produced the slices in the **Ready** state.</span></span>  

![Tillståndet för pipeline](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

<span data-ttu-id="69418-144">Dataset-segment i datafabriken kan ha en av följande status:</span><span class="sxs-lookup"><span data-stu-id="69418-144">The dataset slices in the data factory can have one of the following statuses:</span></span>

<table>
<tr>
    <th align="left"><span data-ttu-id="69418-145">Status</span><span class="sxs-lookup"><span data-stu-id="69418-145">State</span></span></th><th align="left"><span data-ttu-id="69418-146">Undertillstånden</span><span class="sxs-lookup"><span data-stu-id="69418-146">Substate</span></span></th><th align="left"><span data-ttu-id="69418-147">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="69418-147">Description</span></span></th>
</tr>
<tr>
    <td rowspan="8"><span data-ttu-id="69418-148">Väntar</span><span class="sxs-lookup"><span data-stu-id="69418-148">Waiting</span></span></td><td><span data-ttu-id="69418-149">ScheduleTime</span><span class="sxs-lookup"><span data-stu-id="69418-149">ScheduleTime</span></span></td><td><span data-ttu-id="69418-150">Tiden har inte inne för att köra sektorn.</span><span class="sxs-lookup"><span data-stu-id="69418-150">The time hasn't come for the slice to run.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="69418-151">DatasetDependencies</span><span class="sxs-lookup"><span data-stu-id="69418-151">DatasetDependencies</span></span></td><td><span data-ttu-id="69418-152">Uppströmsberoendena är inte redo.</span><span class="sxs-lookup"><span data-stu-id="69418-152">The upstream dependencies aren't ready.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="69418-153">ComputeResources</span><span class="sxs-lookup"><span data-stu-id="69418-153">ComputeResources</span></span></td><td><span data-ttu-id="69418-154">Beräkningsresurserna är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="69418-154">The compute resources aren't available.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="69418-155">ConcurrencyLimit</span><span class="sxs-lookup"><span data-stu-id="69418-155">ConcurrencyLimit</span></span></td> <td><span data-ttu-id="69418-156">Alla aktivitetsinstanserna är upptagna med att köra andra sektorer.</span><span class="sxs-lookup"><span data-stu-id="69418-156">All the activity instances are busy running other slices.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="69418-157">ActivityResume</span><span class="sxs-lookup"><span data-stu-id="69418-157">ActivityResume</span></span></td><td><span data-ttu-id="69418-158">Aktiviteten har pausats och kan inte köra sektorerna förrän aktiviteten återupptas.</span><span class="sxs-lookup"><span data-stu-id="69418-158">The activity is paused and can't run the slices until the activity is resumed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="69418-159">Försök igen</span><span class="sxs-lookup"><span data-stu-id="69418-159">Retry</span></span></td><td><span data-ttu-id="69418-160">Aktivitetskörningen försöks.</span><span class="sxs-lookup"><span data-stu-id="69418-160">Activity execution is being retried.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="69418-161">Validering</span><span class="sxs-lookup"><span data-stu-id="69418-161">Validation</span></span></td><td><span data-ttu-id="69418-162">Verifieringen har inte startat ännu.</span><span class="sxs-lookup"><span data-stu-id="69418-162">Validation hasn't started yet.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="69418-163">ValidationRetry</span><span class="sxs-lookup"><span data-stu-id="69418-163">ValidationRetry</span></span></td><td><span data-ttu-id="69418-164">Verifieringen väntar på att göras.</span><span class="sxs-lookup"><span data-stu-id="69418-164">Validation is waiting to be retried.</span></span></td>
</tr>
<tr>
<tr>
<td rowspan="2"><span data-ttu-id="69418-165">InProgress</span><span class="sxs-lookup"><span data-stu-id="69418-165">InProgress</span></span></td><td><span data-ttu-id="69418-166">Verifiera</span><span class="sxs-lookup"><span data-stu-id="69418-166">Validating</span></span></td><td><span data-ttu-id="69418-167">Verifiering pågår.</span><span class="sxs-lookup"><span data-stu-id="69418-167">Validation is in progress.</span></span></td>
</tr>
<td>-</td>
<td><span data-ttu-id="69418-168">Sektorn behandlas.</span><span class="sxs-lookup"><span data-stu-id="69418-168">The slice is being processed.</span></span></td>
</tr>
<tr>
<td rowspan="4"><span data-ttu-id="69418-169">Det gick inte</span><span class="sxs-lookup"><span data-stu-id="69418-169">Failed</span></span></td><td><span data-ttu-id="69418-170">För lång tid</span><span class="sxs-lookup"><span data-stu-id="69418-170">TimedOut</span></span></td><td><span data-ttu-id="69418-171">Aktivitetskörningen tog längre tid än vad som tillåts av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="69418-171">The activity execution took longer than what is allowed by the activity.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="69418-172">Avbrutna</span><span class="sxs-lookup"><span data-stu-id="69418-172">Canceled</span></span></td><td><span data-ttu-id="69418-173">Sektorn avbröts av en användare.</span><span class="sxs-lookup"><span data-stu-id="69418-173">The slice was canceled by user action.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="69418-174">Validering</span><span class="sxs-lookup"><span data-stu-id="69418-174">Validation</span></span></td><td><span data-ttu-id="69418-175">Verifieringen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="69418-175">Validation has failed.</span></span></td>
</tr>
<tr>
<td>-</td><td><span data-ttu-id="69418-176">Det gick inte att vara genereras och/eller verifiera sektorn.</span><span class="sxs-lookup"><span data-stu-id="69418-176">The slice failed to be generated and/or validated.</span></span></td>
</tr>
<td><span data-ttu-id="69418-177">Redo</span><span class="sxs-lookup"><span data-stu-id="69418-177">Ready</span></span></td><td>-</td><td><span data-ttu-id="69418-178">Sektorn är klar att förbrukas.</span><span class="sxs-lookup"><span data-stu-id="69418-178">The slice is ready for consumption.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="69418-179">Hoppades över</span><span class="sxs-lookup"><span data-stu-id="69418-179">Skipped</span></span></td><td><span data-ttu-id="69418-180">Ingen</span><span class="sxs-lookup"><span data-stu-id="69418-180">None</span></span></td><td><span data-ttu-id="69418-181">Sektorn är inte bearbetas.</span><span class="sxs-lookup"><span data-stu-id="69418-181">The slice isn't being processed.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="69418-182">Ingen</span><span class="sxs-lookup"><span data-stu-id="69418-182">None</span></span></td><td>-</td><td><span data-ttu-id="69418-183">En sektor som brukade finnas med en annan status, men den har återställts.</span><span class="sxs-lookup"><span data-stu-id="69418-183">A slice used to exist with a different status, but it has been reset.</span></span></td>
</tr>
</table>



<span data-ttu-id="69418-184">Du kan visa information om en sektor genom att klicka på en post i sektorn på den **nyligen uppdaterat segment** bladet.</span><span class="sxs-lookup"><span data-stu-id="69418-184">You can view the details about a slice by clicking a slice entry on the **Recently Updated Slices** blade.</span></span>

![Sektorn information](./media/data-factory-monitor-manage-pipelines/slice-details.png)

<span data-ttu-id="69418-186">Om sektorn har körts flera gånger, ser du flera rader i den **aktiviteten körs** lista.</span><span class="sxs-lookup"><span data-stu-id="69418-186">If the slice has been executed multiple times, you see multiple rows in the **Activity runs** list.</span></span> <span data-ttu-id="69418-187">Du kan visa information om en aktivitet som kör genom att klicka på Kör post i den **aktiviteten körs** lista.</span><span class="sxs-lookup"><span data-stu-id="69418-187">You can view details about an activity run by clicking the run entry in the **Activity runs** list.</span></span> <span data-ttu-id="69418-188">I listan visas alla loggfiler, tillsammans med ett felmeddelande om det finns en.</span><span class="sxs-lookup"><span data-stu-id="69418-188">The list shows all the log files, along with an error message if there is one.</span></span> <span data-ttu-id="69418-189">Den här funktionen är användbar för att visa loggar och felsökningsloggar utan att behöva lämna din data factory.</span><span class="sxs-lookup"><span data-stu-id="69418-189">This feature is useful to view and debug logs without having to leave your data factory.</span></span>

![Aktivitetskörningsinformation](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

<span data-ttu-id="69418-191">Om sektorn inte i den **klar** tillstånd, kan du se sektorer uppströms som inte är klar och blockerar aktuellt segment från att köras i den **sektorer uppströms som inte är redo** lista.</span><span class="sxs-lookup"><span data-stu-id="69418-191">If the slice isn't in the **Ready** state, you can see the upstream slices that aren't ready and are blocking the current slice from executing in the **Upstream slices that are not ready** list.</span></span> <span data-ttu-id="69418-192">Den här funktionen är användbart när din sektorn är i **väntar på** tillstånd och du vill förstå uppströmsberoendena sektorn väntar på.</span><span class="sxs-lookup"><span data-stu-id="69418-192">This feature is useful when your slice is in **Waiting** state and you want to understand the upstream dependencies that the slice is waiting on.</span></span>

![Sektorer uppströms som inte är klara](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a><span data-ttu-id="69418-194">DataSet tillståndsdiagram</span><span class="sxs-lookup"><span data-stu-id="69418-194">Dataset state diagram</span></span>
<span data-ttu-id="69418-195">När du distribuerar en datafabrik och pipelines har en ogiltig aktiv period, sektorer datauppsättningen övergång från ett tillstånd till en annan.</span><span class="sxs-lookup"><span data-stu-id="69418-195">After you deploy a data factory and the pipelines have a valid active period, the dataset slices transition from one state to another.</span></span> <span data-ttu-id="69418-196">För närvarande följande sektorn status i följande tillståndsdiagram:</span><span class="sxs-lookup"><span data-stu-id="69418-196">Currently, the slice status follows the following state diagram:</span></span>

![Tillståndsdiagram](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

<span data-ttu-id="69418-198">Dataset tillstånd övergången flödet i data factory är följande: väntar -> förlopp/i-pågående (verifierar) -> klar/misslyckades.</span><span class="sxs-lookup"><span data-stu-id="69418-198">The dataset state transition flow in data factory is the following: Waiting -> In-Progress/In-Progress (Validating) -> Ready/Failed.</span></span>

<span data-ttu-id="69418-199">Sektorn startar i en **väntar på** tillstånd, väntar på villkor som måste uppfyllas innan den körs.</span><span class="sxs-lookup"><span data-stu-id="69418-199">The slice starts in a **Waiting** state, waiting for preconditions to be met before it executes.</span></span> <span data-ttu-id="69418-200">Sedan aktiviteten startar körning och sektorn försätts i ett **pågående** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="69418-200">Then, the activity starts executing, and the slice goes into an **In-Progress** state.</span></span> <span data-ttu-id="69418-201">Aktivitetskörningen kan lyckas eller misslyckas.</span><span class="sxs-lookup"><span data-stu-id="69418-201">The activity execution might succeed or fail.</span></span> <span data-ttu-id="69418-202">Sektorn är markerad som **klar** eller **misslyckades**, baserat på resultatet av körningen.</span><span class="sxs-lookup"><span data-stu-id="69418-202">The slice is marked as **Ready** or **Failed**, based on the result of the execution.</span></span>

<span data-ttu-id="69418-203">Du kan återställa sektorn gå tillbaka från den **klar** eller **misslyckades** till den **väntar på** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="69418-203">You can reset the slice to go back from the **Ready** or **Failed** state to the **Waiting** state.</span></span> <span data-ttu-id="69418-204">Du kan också markera segment läget till **hoppa över**, vilket förhindrar att aktiviteten körs och icke-bearbetning av sektorn.</span><span class="sxs-lookup"><span data-stu-id="69418-204">You can also mark the slice state to **Skip**, which prevents the activity from executing and not processing the slice.</span></span>

## <a name="pause-and-resume-pipelines"></a><span data-ttu-id="69418-205">Pausa och återuppta pipelines</span><span class="sxs-lookup"><span data-stu-id="69418-205">Pause and resume pipelines</span></span>
<span data-ttu-id="69418-206">Du kan hantera dina pipelines med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="69418-206">You can manage your pipelines by using Azure PowerShell.</span></span> <span data-ttu-id="69418-207">Du kan till exempel pausa och återuppta pipelines genom att köra Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="69418-207">For example, you can pause and resume pipelines by running Azure PowerShell cmdlets.</span></span> 

> [!NOTE] 
> <span data-ttu-id="69418-208">Diagramvyn stöder inte pausa och återuppta pipelines.</span><span class="sxs-lookup"><span data-stu-id="69418-208">The diagram view does not support pausing and resuming pipelines.</span></span> <span data-ttu-id="69418-209">Använd övervaknings- och hantera program om du vill använda ett användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="69418-209">If you want to use an user interface, use the monitoring and managing application.</span></span> <span data-ttu-id="69418-210">Mer information om hur du använder programmet finns [övervaka och hantera Data Factory pipelines med hjälp av övervakning och hantering av appen](data-factory-monitor-manage-app.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="69418-210">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

<span data-ttu-id="69418-211">Du kan Pausa/Pausa pipelines med hjälp av den **pausa AzureRmDataFactoryPipeline** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="69418-211">You can pause/suspend pipelines by using the **Suspend-AzureRmDataFactoryPipeline** PowerShell cmdlet.</span></span> <span data-ttu-id="69418-212">Denna cmdlet är användbar när du inte vill köra din pipelines förrän problemet har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="69418-212">This cmdlet is useful when you don’t want to run your pipelines until an issue is fixed.</span></span> 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="69418-213">Exempel:</span><span class="sxs-lookup"><span data-stu-id="69418-213">For example:</span></span>

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

<span data-ttu-id="69418-214">När problemet har åtgärdats med pipeline, kan du återuppta pausade pipeline genom att köra följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="69418-214">After the issue has been fixed with the pipeline, you can resume the suspended pipeline by running the following PowerShell command:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
<span data-ttu-id="69418-215">Exempel:</span><span class="sxs-lookup"><span data-stu-id="69418-215">For example:</span></span>

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a><span data-ttu-id="69418-216">Felsöka pipelines</span><span class="sxs-lookup"><span data-stu-id="69418-216">Debug pipelines</span></span>
<span data-ttu-id="69418-217">Azure Data Factory ger omfattande funktioner för dig att felsöka pipelines med hjälp av Azure-portalen och Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="69418-217">Azure Data Factory provides rich capabilities for you to debug and troubleshoot pipelines by using the Azure portal and Azure PowerShell.</span></span>

> <span data-ttu-id="69418-218">[! Observera} är det mycket enklare Felsökte fel med övervakning & Management-appen.</span><span class="sxs-lookup"><span data-stu-id="69418-218">[!NOTE} It is much easier to troubleshot errors using the Monitoring & Management App.</span></span> <span data-ttu-id="69418-219">Mer information om hur du använder programmet finns [övervaka och hantera Data Factory pipelines med hjälp av övervakning och hantering av appen](data-factory-monitor-manage-app.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="69418-219">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md) article.</span></span> 

### <a name="find-errors-in-a-pipeline"></a><span data-ttu-id="69418-220">Sök efter fel i en pipeline</span><span class="sxs-lookup"><span data-stu-id="69418-220">Find errors in a pipeline</span></span>
<span data-ttu-id="69418-221">Om aktiviteten körs inte i en pipeline, är dataset som produceras av pipelinen i ett feltillstånd på grund av felet.</span><span class="sxs-lookup"><span data-stu-id="69418-221">If the activity run fails in a pipeline, the dataset that is produced by the pipeline is in an error state because of the failure.</span></span> <span data-ttu-id="69418-222">Du kan felsöka och felsöka i Azure Data Factory med hjälp av följande metoder.</span><span class="sxs-lookup"><span data-stu-id="69418-222">You can debug and troubleshoot errors in Azure Data Factory by using the following methods.</span></span>

#### <a name="use-the-azure-portal-to-debug-an-error"></a><span data-ttu-id="69418-223">Använda Azure portal för att felsöka ett fel</span><span class="sxs-lookup"><span data-stu-id="69418-223">Use the Azure portal to debug an error</span></span>
1. <span data-ttu-id="69418-224">På den **tabell** bladet på problemet sektor som har det **Status** inställd på **misslyckades**.</span><span class="sxs-lookup"><span data-stu-id="69418-224">On the **Table** blade, click the problem slice that has the **Status** set to **Failed**.</span></span>

   ![Tabell bladet med problem](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. <span data-ttu-id="69418-226">På den **datasektorn** bladet, klickar du på aktiviteten körs som misslyckades.</span><span class="sxs-lookup"><span data-stu-id="69418-226">On the **Data slice** blade, click the activity run that failed.</span></span>

   ![Datasektorn med ett fel](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. <span data-ttu-id="69418-228">På den **aktivitet köras information** bladet som du kan hämta de filer som är associerade med HDInsight-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="69418-228">On the **Activity run details** blade, you can download the files that are associated with the HDInsight processing.</span></span> <span data-ttu-id="69418-229">Klicka på **hämta** för Status/stderr att ladda ned felloggen som innehåller information om felet.</span><span class="sxs-lookup"><span data-stu-id="69418-229">Click **Download** for Status/stderr to download the error log file that contains details about the error.</span></span>

   ![Aktiviteten kör informationsbladet med fel](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-to-debug-an-error"></a><span data-ttu-id="69418-231">Använd PowerShell för att felsöka ett fel</span><span class="sxs-lookup"><span data-stu-id="69418-231">Use PowerShell to debug an error</span></span>
1. <span data-ttu-id="69418-232">Starta **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="69418-232">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="69418-233">Kör den **Get-AzureRmDataFactorySlice** kommandot för att se sektorerna och deras status.</span><span class="sxs-lookup"><span data-stu-id="69418-233">Run the **Get-AzureRmDataFactorySlice** command to see the slices and their statuses.</span></span> <span data-ttu-id="69418-234">Du bör se ett segment med statusen **misslyckades**.</span><span class="sxs-lookup"><span data-stu-id="69418-234">You should see a slice with the status of **Failed**.</span></span>        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   <span data-ttu-id="69418-235">Exempel:</span><span class="sxs-lookup"><span data-stu-id="69418-235">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   <span data-ttu-id="69418-236">Ersätt **StartDateTime** med starttiden för din pipeline.</span><span class="sxs-lookup"><span data-stu-id="69418-236">Replace **StartDateTime** with start time of your pipeline.</span></span> 
3. <span data-ttu-id="69418-237">Kör nu den **Get-AzureRmDataFactoryRun** för att hämta information om aktiviteten kör för sektorn.</span><span class="sxs-lookup"><span data-stu-id="69418-237">Now, run the **Get-AzureRmDataFactoryRun** cmdlet to get details about the activity run for the slice.</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    <span data-ttu-id="69418-238">Exempel:</span><span class="sxs-lookup"><span data-stu-id="69418-238">For example:</span></span>

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    <span data-ttu-id="69418-239">Värdet för StartDateTime är starttiden för sektorn fel/problem som du antecknade från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="69418-239">The value of StartDateTime is the start time for the error/problem slice that you noted from the previous step.</span></span> <span data-ttu-id="69418-240">Datum och tid ska omges av dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="69418-240">The date-time should be enclosed in double quotes.</span></span>
4. <span data-ttu-id="69418-241">Du bör se utdata med information om felet som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="69418-241">You should see output with details about the error that is similar to the following:</span></span>

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
5. <span data-ttu-id="69418-242">Du kan köra den **spara AzureRmDataFactoryLog** med ID-värde som du ser från utdata och hämta filerna med hjälp av den **- DownloadLogsoption** för cmdleten.</span><span class="sxs-lookup"><span data-stu-id="69418-242">You can run the **Save-AzureRmDataFactoryLog** cmdlet with the Id value that you see from the output, and download the log files by using the **-DownloadLogsoption** for the cmdlet.</span></span>

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a><span data-ttu-id="69418-243">Kör fel i en pipeline</span><span class="sxs-lookup"><span data-stu-id="69418-243">Rerun failures in a pipeline</span></span>

> [!IMPORTANT]
> <span data-ttu-id="69418-244">Det är enklare att felsöka och kör misslyckade segment med hjälp av övervakning & Management-appen.</span><span class="sxs-lookup"><span data-stu-id="69418-244">It's easier to troubleshoot errors and rerun failed slices by using the Monitoring & Management App.</span></span> <span data-ttu-id="69418-245">Mer information om hur du använder programmet finns [övervaka och hantera Data Factory pipelines med hjälp av övervakning och hantering av appen](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="69418-245">For details about using the application, see [monitor and manage Data Factory pipelines by using the Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span> 

### <a name="use-the-azure-portal"></a><span data-ttu-id="69418-246">Använda Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="69418-246">Use the Azure portal</span></span>
<span data-ttu-id="69418-247">När du felsöker och felsöka fel i en pipeline, kan du köra fel genom att gå till fel segment och klicka på den **kör** knappen i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="69418-247">After you troubleshoot and debug failures in a pipeline, you can rerun failures by navigating to the error slice and clicking the **Run** button on the command bar.</span></span>

![Kör en misslyckad sektor](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

<span data-ttu-id="69418-249">I fallet sektorn valideringen misslyckades på grund av en princip-fel (till exempel om data inte är tillgängligt), kan du åtgärda felet och validera igen genom att klicka på den **verifiera** knappen i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="69418-249">In case the slice has failed validation because of a policy failure (for example, if data isn't available), you can fix the failure and validate again by clicking the **Validate** button on the command bar.</span></span>

![Åtgärda felen och validera](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a><span data-ttu-id="69418-251">Använda Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="69418-251">Use Azure PowerShell</span></span>
<span data-ttu-id="69418-252">Du kan köra fel med hjälp av den **Set AzureRmDataFactorySliceStatus** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="69418-252">You can rerun failures by using the **Set-AzureRmDataFactorySliceStatus** cmdlet.</span></span> <span data-ttu-id="69418-253">Finns det [Set AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) avsnittet annan information om cmdlet och syntax.</span><span class="sxs-lookup"><span data-stu-id="69418-253">See the [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) topic for syntax and other details about the cmdlet.</span></span>

<span data-ttu-id="69418-254">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="69418-254">**Example:**</span></span>

<span data-ttu-id="69418-255">I följande exempel anger status för alla segment för tabellen 'DAWikiAggregatedData' till väntar i Azure data factory 'WikiADF'.</span><span class="sxs-lookup"><span data-stu-id="69418-255">The following example sets the status of all slices for the table 'DAWikiAggregatedData' to 'Waiting' in the Azure data factory 'WikiADF'.</span></span>

<span data-ttu-id="69418-256">Uppdateringstyp är inställd på 'UpstreamInPipeline', vilket innebär att statusen för varje segment för tabellen och alla beroende (överordnad) tabeller är inställt på ”väntande”.</span><span class="sxs-lookup"><span data-stu-id="69418-256">The 'UpdateType' is set to 'UpstreamInPipeline', which means that statuses of each slice for the table and all the dependent (upstream) tables are set to 'Waiting'.</span></span> <span data-ttu-id="69418-257">Andra möjliga värde för den här parametern är 'Person'.</span><span class="sxs-lookup"><span data-stu-id="69418-257">The other possible value for this parameter is 'Individual'.</span></span>

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```

## <a name="create-alerts"></a><span data-ttu-id="69418-258">Skapa aviseringar</span><span class="sxs-lookup"><span data-stu-id="69418-258">Create alerts</span></span>
<span data-ttu-id="69418-259">Användaren Azure loggar händelser när en Azure-resurs (till exempel data factory) skapas, uppdateras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="69418-259">Azure logs user events when an Azure resource (for example, a data factory) is created, updated, or deleted.</span></span> <span data-ttu-id="69418-260">Du kan skapa aviseringar på dessa händelser.</span><span class="sxs-lookup"><span data-stu-id="69418-260">You can create alerts on these events.</span></span> <span data-ttu-id="69418-261">Du kan använda Data Factory för att samla in olika mått och skapa aviseringar på mått.</span><span class="sxs-lookup"><span data-stu-id="69418-261">You can use Data Factory to capture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="69418-262">Vi rekommenderar att du använder händelser för övervakning i realtid och använda mått i historiksyfte.</span><span class="sxs-lookup"><span data-stu-id="69418-262">We recommend that you use events for real-time monitoring and use metrics for historical purposes.</span></span>

### <a name="alerts-on-events"></a><span data-ttu-id="69418-263">Aviseringar om händelser</span><span class="sxs-lookup"><span data-stu-id="69418-263">Alerts on events</span></span>
<span data-ttu-id="69418-264">Azure händelser ger användbar insikter om vad som händer i Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="69418-264">Azure events provide useful insights into what is happening in your Azure resources.</span></span> <span data-ttu-id="69418-265">När du använder Azure Data Factory händelser genereras när:</span><span class="sxs-lookup"><span data-stu-id="69418-265">When you're using Azure Data Factory, events are generated when:</span></span>

* <span data-ttu-id="69418-266">En datafabrik har skapats, uppdateras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="69418-266">A data factory is created, updated, or deleted.</span></span>
* <span data-ttu-id="69418-267">Databearbetning (som ”kör”) har startats eller slutförts.</span><span class="sxs-lookup"><span data-stu-id="69418-267">Data processing (as "runs") has started or completed.</span></span>
* <span data-ttu-id="69418-268">Ett HDInsight-kluster på begäran skapas eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="69418-268">An on-demand HDInsight cluster is created or removed.</span></span>

<span data-ttu-id="69418-269">Du kan skapa varningar på dessa användarhändelser och konfigurera dem för att skicka e-postaviseringar till administratören och coadministrators för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="69418-269">You can create alerts on these user events and configure them to send email notifications to the administrator and coadministrators of the subscription.</span></span> <span data-ttu-id="69418-270">Dessutom kan du ange ytterligare e-postadresserna för användare som behöver ta emot e-postaviseringar när villkoren är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="69418-270">In addition, you can specify additional email addresses of users who need to receive email notifications when the conditions are met.</span></span> <span data-ttu-id="69418-271">Den här funktionen är användbart när du vill få meddelanden på fel och inte vill att ständigt övervaka din data factory.</span><span class="sxs-lookup"><span data-stu-id="69418-271">This feature is useful when you want to get notified on failures and don’t want to continuously monitor your data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="69418-272">För närvarande visar portalen inte aviseringar om händelser.</span><span class="sxs-lookup"><span data-stu-id="69418-272">Currently, the portal doesn't show alerts on events.</span></span> <span data-ttu-id="69418-273">Använd den [övervakning och hantering av](data-factory-monitor-manage-app.md) att se alla varningar.</span><span class="sxs-lookup"><span data-stu-id="69418-273">Use the [Monitoring and Management app](data-factory-monitor-manage-app.md) to see all alerts.</span></span>


#### <a name="specify-an-alert-definition"></a><span data-ttu-id="69418-274">Ange en definition av varning</span><span class="sxs-lookup"><span data-stu-id="69418-274">Specify an alert definition</span></span>
<span data-ttu-id="69418-275">Om du vill ange en definition av aviseringen, kan du skapa en JSON-fil som beskriver de åtgärder som du vill bli aviserad om.</span><span class="sxs-lookup"><span data-stu-id="69418-275">To specify an alert definition, you create a JSON file that describes the operations that you want to be alerted on.</span></span> <span data-ttu-id="69418-276">I följande exempel skickar aviseringen ett e-postmeddelande för RunFinished igen.</span><span class="sxs-lookup"><span data-stu-id="69418-276">In the following example, the alert sends an email notification for the RunFinished operation.</span></span> <span data-ttu-id="69418-277">Om du vill att specifika skickas ett e-postmeddelande när en körning i datafabriken har slutförts och kör misslyckades (Status = FailedExecution).</span><span class="sxs-lookup"><span data-stu-id="69418-277">To be specific, an email notification is sent when a run in the data factory has completed and the run has failed (Status = FailedExecution).</span></span>

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
                "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
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

<span data-ttu-id="69418-278">Du kan ta bort **subStatus** från JSON-definitionen om du inte vill bli aviserad om ett specifikt fel.</span><span class="sxs-lookup"><span data-stu-id="69418-278">You can remove **subStatus** from the JSON definition if you don’t want to be alerted on a specific failure.</span></span>

<span data-ttu-id="69418-279">Det här exemplet konfigurerar varningen för alla datafabriker i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="69418-279">This example sets up the alert for all data factories in your subscription.</span></span> <span data-ttu-id="69418-280">Om du vill att aviseringen ska ställas in för en viss datafabrik, kan du ange data factory **resourceUri** i den **dataSource**:</span><span class="sxs-lookup"><span data-stu-id="69418-280">If you want the alert to be set up for a particular data factory, you can specify data factory **resourceUri** in the **dataSource**:</span></span>

```JSON
"resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"
```

<span data-ttu-id="69418-281">Följande tabell innehåller en lista över tillgängliga åtgärder och status (och underordnad status).</span><span class="sxs-lookup"><span data-stu-id="69418-281">The following table provides the list of available operations and statuses (and substatuses).</span></span>

| <span data-ttu-id="69418-282">Åtgärdsnamn</span><span class="sxs-lookup"><span data-stu-id="69418-282">Operation name</span></span> | <span data-ttu-id="69418-283">Status</span><span class="sxs-lookup"><span data-stu-id="69418-283">Status</span></span> | <span data-ttu-id="69418-284">Substatus</span><span class="sxs-lookup"><span data-stu-id="69418-284">Substatus</span></span> |
| --- | --- | --- |
| <span data-ttu-id="69418-285">RunStarted</span><span class="sxs-lookup"><span data-stu-id="69418-285">RunStarted</span></span> |<span data-ttu-id="69418-286">Igång</span><span class="sxs-lookup"><span data-stu-id="69418-286">Started</span></span> |<span data-ttu-id="69418-287">Startar</span><span class="sxs-lookup"><span data-stu-id="69418-287">Starting</span></span> |
| <span data-ttu-id="69418-288">RunFinished</span><span class="sxs-lookup"><span data-stu-id="69418-288">RunFinished</span></span> |<span data-ttu-id="69418-289">Misslyckades / lyckades</span><span class="sxs-lookup"><span data-stu-id="69418-289">Failed / Succeeded</span></span> |<span data-ttu-id="69418-290">FailedResourceAllocation</span><span class="sxs-lookup"><span data-stu-id="69418-290">FailedResourceAllocation</span></span><br/><br/><span data-ttu-id="69418-291">Lyckades</span><span class="sxs-lookup"><span data-stu-id="69418-291">Succeeded</span></span><br/><br/><span data-ttu-id="69418-292">FailedExecution</span><span class="sxs-lookup"><span data-stu-id="69418-292">FailedExecution</span></span><br/><br/><span data-ttu-id="69418-293">För lång tid</span><span class="sxs-lookup"><span data-stu-id="69418-293">TimedOut</span></span><br/><br/><span data-ttu-id="69418-294">< avbröts</span><span class="sxs-lookup"><span data-stu-id="69418-294"><Canceled</span></span><br/><br/><span data-ttu-id="69418-295">FailedValidation</span><span class="sxs-lookup"><span data-stu-id="69418-295">FailedValidation</span></span><br/><br/><span data-ttu-id="69418-296">Avbrutna</span><span class="sxs-lookup"><span data-stu-id="69418-296">Abandoned</span></span> |
| <span data-ttu-id="69418-297">OnDemandClusterCreateStarted</span><span class="sxs-lookup"><span data-stu-id="69418-297">OnDemandClusterCreateStarted</span></span> |<span data-ttu-id="69418-298">Igång</span><span class="sxs-lookup"><span data-stu-id="69418-298">Started</span></span> | |
| <span data-ttu-id="69418-299">OnDemandClusterCreateSuccessful</span><span class="sxs-lookup"><span data-stu-id="69418-299">OnDemandClusterCreateSuccessful</span></span> |<span data-ttu-id="69418-300">Lyckades</span><span class="sxs-lookup"><span data-stu-id="69418-300">Succeeded</span></span> | |
| <span data-ttu-id="69418-301">OnDemandClusterDeleted</span><span class="sxs-lookup"><span data-stu-id="69418-301">OnDemandClusterDeleted</span></span> |<span data-ttu-id="69418-302">Lyckades</span><span class="sxs-lookup"><span data-stu-id="69418-302">Succeeded</span></span> | |

<span data-ttu-id="69418-303">Se [skapa Varningsregeln](https://msdn.microsoft.com/library/azure/dn510366.aspx) för ytterligare information om JSON-element som används i exemplet.</span><span class="sxs-lookup"><span data-stu-id="69418-303">See [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) for details about the JSON elements that are used in the example.</span></span>

#### <a name="deploy-the-alert"></a><span data-ttu-id="69418-304">Distribuera aviseringen</span><span class="sxs-lookup"><span data-stu-id="69418-304">Deploy the alert</span></span>
<span data-ttu-id="69418-305">Använda Azure PowerShell-cmdlet för att distribuera aviseringen **ny AzureRmResourceGroupDeployment**som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="69418-305">To deploy the alert, use the Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in the following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  
```

<span data-ttu-id="69418-306">När resursen gruppdistributionen är klar visas följande meddelanden:</span><span class="sxs-lookup"><span data-stu-id="69418-306">After the resource group deployment has finished successfully, you see the following messages:</span></span>

```
VERBOSE: 7:00:48 PM - Template is valid.
WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
Please update scripts to remove this parameter.
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
> <span data-ttu-id="69418-307">Du kan använda den [Skapa regel för varning](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API för att skapa en aviseringsregel.</span><span class="sxs-lookup"><span data-stu-id="69418-307">You can use the [Create Alert Rule](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API to create an alert rule.</span></span> <span data-ttu-id="69418-308">JSON-nyttolast liknar JSON-exemplet.</span><span class="sxs-lookup"><span data-stu-id="69418-308">The JSON payload is similar to the JSON example.</span></span>  


#### <a name="retrieve-the-list-of-azure-resource-group-deployments"></a><span data-ttu-id="69418-309">Hämta listan över distributionen av Azure resursgrupper</span><span class="sxs-lookup"><span data-stu-id="69418-309">Retrieve the list of Azure resource group deployments</span></span>
<span data-ttu-id="69418-310">Om du vill hämta listan över distributionen av distribuerade Azure resursgrupper, använder du cmdlet **Get-AzureRmResourceGroupDeployment**som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="69418-310">To retrieve the list of deployed Azure resource group deployments, use the cmdlet **Get-AzureRmResourceGroupDeployment**, as shown in the following example:</span></span>

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

#### <a name="troubleshoot-user-events"></a><span data-ttu-id="69418-311">Felsöka användarhändelser</span><span class="sxs-lookup"><span data-stu-id="69418-311">Troubleshoot user events</span></span>
1. <span data-ttu-id="69418-312">Du kan se alla händelser som genereras när du klickar på den **mått och åtgärder** panelen.</span><span class="sxs-lookup"><span data-stu-id="69418-312">You can see all the events that are generated after clicking the **Metrics and operations** tile.</span></span>

    ![Panelen mått och åtgärder](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)
2. <span data-ttu-id="69418-314">Klicka på den **händelser** rutan för att se händelserna.</span><span class="sxs-lookup"><span data-stu-id="69418-314">Click the **Events** tile to see the events.</span></span>

    ![Panelen händelser](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. <span data-ttu-id="69418-316">På den **händelser** bladet hittar du information om händelser, filtreras händelser och så vidare.</span><span class="sxs-lookup"><span data-stu-id="69418-316">On the **Events** blade, you can see details about events, filtered events, and so on.</span></span>

    ![Händelser bladet](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. <span data-ttu-id="69418-318">Klicka på en **åtgärden** i listan över funktioner som orsakar ett fel.</span><span class="sxs-lookup"><span data-stu-id="69418-318">Click an **Operation** in the operations list that causes an error.</span></span>

    ![Välj en åtgärd](./media/data-factory-monitor-manage-pipelines/select-operation.png)
5. <span data-ttu-id="69418-320">Klicka på en **fel** händelsen för att se information om felet.</span><span class="sxs-lookup"><span data-stu-id="69418-320">Click an **Error** event to see details about the error.</span></span>

    ![Händelsefel](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)

<span data-ttu-id="69418-322">Se [Azure Insight cmdlets](https://msdn.microsoft.com/library/mt282452.aspx) hämta för PowerShell-cmdlets som du kan använda för att lägga till eller ta bort aviseringar.</span><span class="sxs-lookup"><span data-stu-id="69418-322">See [Azure Insight cmdlets](https://msdn.microsoft.com/library/mt282452.aspx) for PowerShell cmdlets that you can use to add, get, or remove alerts.</span></span> <span data-ttu-id="69418-323">Här följer några exempel på användning av den **Get-AlertRule** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="69418-323">Here are a few examples of using the **Get-AlertRule** cmdlet:</span></span>

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
Description : One or more of the data slices for the Azure Data Factory has failed processing.
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

<span data-ttu-id="69418-324">Kör följande kommandon för get-help att se information och exempel för att cmdleten Get-AlertRule.</span><span class="sxs-lookup"><span data-stu-id="69418-324">Run the following get-help commands to see details and examples for the Get-AlertRule cmdlet.</span></span>

```powershell
get-help Get-AlertRule -detailed
```

```powershell
get-help Get-AlertRule -examples
```


<span data-ttu-id="69418-325">Om du ser varningsgenereringen händelser på portalbladet men du inte får e-postmeddelanden, kontrollera om e-postadressen som anges har angetts för att ta emot e-post från externa avsändare.</span><span class="sxs-lookup"><span data-stu-id="69418-325">If you see the alert generation events on the portal blade but you don't receive email notifications, check whether the email address that is specified is set to receive emails from external senders.</span></span> <span data-ttu-id="69418-326">Meddelandena kanske har blockerats av din e-postinställningar.</span><span class="sxs-lookup"><span data-stu-id="69418-326">The alert emails might have been blocked by your email settings.</span></span>

### <a name="alerts-on-metrics"></a><span data-ttu-id="69418-327">Aviseringar om mått</span><span class="sxs-lookup"><span data-stu-id="69418-327">Alerts on metrics</span></span>
<span data-ttu-id="69418-328">I Data Factory du samla in olika mått och skapa aviseringar på mått.</span><span class="sxs-lookup"><span data-stu-id="69418-328">In Data Factory, you can capture various metrics and create alerts on metrics.</span></span> <span data-ttu-id="69418-329">Du kan övervaka och skapa aviseringar på följande mätvärden för sektorerna i din data factory:</span><span class="sxs-lookup"><span data-stu-id="69418-329">You can monitor and create alerts on the following metrics for the slices in your data factory:</span></span>

* <span data-ttu-id="69418-330">**Misslyckade körs**</span><span class="sxs-lookup"><span data-stu-id="69418-330">**Failed Runs**</span></span>
* <span data-ttu-id="69418-331">**Lyckad körs**</span><span class="sxs-lookup"><span data-stu-id="69418-331">**Successful Runs**</span></span>

<span data-ttu-id="69418-332">De här måtten är användbara och hjälper dig att få en översikt över övergripande misslyckade och lyckade körs i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="69418-332">These metrics are useful and help you to get an overview of overall failed and successful runs in the data factory.</span></span> <span data-ttu-id="69418-333">Mått är orsakat varje gång det är ett segment kör.</span><span class="sxs-lookup"><span data-stu-id="69418-333">Metrics are emitted every time there is a slice run.</span></span> <span data-ttu-id="69418-334">I början av en timme, är de här måtten samman och pushas till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="69418-334">At the beginning of the hour, these metrics are aggregated and pushed to your storage account.</span></span> <span data-ttu-id="69418-335">Om du vill aktivera mätvärden, ställa in ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="69418-335">To enable metrics, set up a storage account.</span></span>

#### <a name="enable-metrics"></a><span data-ttu-id="69418-336">Aktivera mätvärden</span><span class="sxs-lookup"><span data-stu-id="69418-336">Enable metrics</span></span>
<span data-ttu-id="69418-337">Om du vill aktivera mätvärden, klickar du på följande från den **Data Factory** bladet:</span><span class="sxs-lookup"><span data-stu-id="69418-337">To enable metrics, click the following from the **Data Factory** blade:</span></span>

<span data-ttu-id="69418-338">**Övervaka** > **mått** > **diagnostikinställningar** > **diagnostik**</span><span class="sxs-lookup"><span data-stu-id="69418-338">**Monitoring** > **Metric** > **Diagnostic settings** > **Diagnostics**</span></span>

![Diagnostik-länk](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

<span data-ttu-id="69418-340">På den **diagnostik** bladet, klickar du på **på**, Välj lagringskonto och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="69418-340">On the **Diagnostics** blade, click **On**, select the storage account, and click **Save**.</span></span>

![Diagnostik-bladet](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

<span data-ttu-id="69418-342">Det kan ta upp till en timme för de mätvärden som ska visas på den **övervakning** bladet eftersom mått aggregering sker varje timme.</span><span class="sxs-lookup"><span data-stu-id="69418-342">It might take up to one hour for the metrics to be visible on the **Monitoring** blade because metrics aggregation happens hourly.</span></span>

### <a name="set-up-an-alert-on-metrics"></a><span data-ttu-id="69418-343">Skapa en avisering om mått</span><span class="sxs-lookup"><span data-stu-id="69418-343">Set up an alert on metrics</span></span>
<span data-ttu-id="69418-344">Klicka på den **Data Factory mått** panelen:</span><span class="sxs-lookup"><span data-stu-id="69418-344">Click the **Data Factory metrics** tile:</span></span>

![Data factory mått sida vid sida](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

<span data-ttu-id="69418-346">På den **mått** bladet, klickar du på **+ Lägg till avisering** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="69418-346">On the **Metric** blade, click **+ Add alert** on the toolbar.</span></span>
<span data-ttu-id="69418-347">![Data factory mått bladet > Lägg till avisering](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span><span class="sxs-lookup"><span data-stu-id="69418-347">![Data factory metric blade > Add alert](./media/data-factory-monitor-manage-pipelines/add-alert.png)</span></span>

<span data-ttu-id="69418-348">På den **lägga till en varningsregel** , gör du följande och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="69418-348">On the **Add an alert rule** page, do the following steps, and click **OK**.</span></span>

* <span data-ttu-id="69418-349">Ange ett namn för aviseringen (exempel: ”misslyckades aviseringen”).</span><span class="sxs-lookup"><span data-stu-id="69418-349">Enter a name for the alert (example: "failed alert").</span></span>
* <span data-ttu-id="69418-350">Ange en beskrivning för aviseringen (exempel: ”skicka ett e-postmeddelande när ett fel uppstår”).</span><span class="sxs-lookup"><span data-stu-id="69418-350">Enter a description for the alert (example: "send an email when a failure occurs").</span></span>
* <span data-ttu-id="69418-351">Markera ett mått (vs ”kunde inte körs”. ”Lyckades körs”).</span><span class="sxs-lookup"><span data-stu-id="69418-351">Select a metric ("Failed Runs" vs. "Successful Runs").</span></span>
* <span data-ttu-id="69418-352">Ange ett villkor och ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="69418-352">Specify a condition and a threshold value.</span></span>   
* <span data-ttu-id="69418-353">Ange tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="69418-353">Specify the period of time.</span></span>
* <span data-ttu-id="69418-354">Ange om ett e-postmeddelande ska skickas till ägare, deltagare och läsare.</span><span class="sxs-lookup"><span data-stu-id="69418-354">Specify whether an email should be sent to owners, contributors, and readers.</span></span>

![Data factory mått bladet > Lägg till regel för varning](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

<span data-ttu-id="69418-356">När regeln har lagts till, bladet stängs och du ser den nya aviseringen på den **mått** bladet.</span><span class="sxs-lookup"><span data-stu-id="69418-356">After the alert rule is added successfully, the blade closes and you see the new alert on the **Metric** blade.</span></span>

![Data factory mått bladet > Ny avisering som lagts till](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

<span data-ttu-id="69418-358">Du bör också se antalet aviseringar i den **Varna regler** panelen.</span><span class="sxs-lookup"><span data-stu-id="69418-358">You should also see the number of alerts in the **Alert rules** tile.</span></span> <span data-ttu-id="69418-359">Klicka på den **Varna regler** panelen.</span><span class="sxs-lookup"><span data-stu-id="69418-359">Click the **Alert rules** tile.</span></span>

![Data factory mått-bladet – Varningsregler](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

<span data-ttu-id="69418-361">På den **aviseringar regler** bladet visas alla befintliga varningar.</span><span class="sxs-lookup"><span data-stu-id="69418-361">On the **Alerts rules** blade, you see any existing alerts.</span></span> <span data-ttu-id="69418-362">Om du vill lägga till en avisering klickar du på **Lägg till avisering** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="69418-362">To add an alert, click **Add alert** on the toolbar.</span></span>

![Varningsregler bladet](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a><span data-ttu-id="69418-364">Varningsmeddelanden</span><span class="sxs-lookup"><span data-stu-id="69418-364">Alert notifications</span></span>
<span data-ttu-id="69418-365">När regeln matchar villkoret, bör du få ett e-postmeddelande som säger att aviseringen har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="69418-365">After the alert rule matches the condition, you should get an email that says the alert is activated.</span></span> <span data-ttu-id="69418-366">När problemet är löst och aviseringstillståndet matchar inte längre kan få du ett e-postmeddelande att aviseringen har lösts.</span><span class="sxs-lookup"><span data-stu-id="69418-366">After the issue is resolved and the alert condition doesn’t match anymore, you get an email that says the alert is resolved.</span></span>

<span data-ttu-id="69418-367">Det här skiljer sig händelser där skickas ett meddelande på alla fel som en aviseringsregel är berättigad till.</span><span class="sxs-lookup"><span data-stu-id="69418-367">This behavior is different than events where a notification is sent on every failure that an alert rule qualifies for.</span></span>

### <a name="deploy-alerts-by-using-powershell"></a><span data-ttu-id="69418-368">Distribuera aviseringar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="69418-368">Deploy alerts by using PowerShell</span></span>
<span data-ttu-id="69418-369">Du kan distribuera aviseringar för mått på samma sätt som du gör för händelser.</span><span class="sxs-lookup"><span data-stu-id="69418-369">You can deploy alerts for metrics the same way that you do for events.</span></span>

<span data-ttu-id="69418-370">**Varningsdefinitionen**</span><span class="sxs-lookup"><span data-stu-id="69418-370">**Alert definition**</span></span>

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

<span data-ttu-id="69418-371">Ersätt *subscriptionId*, *resourceGroupName*, och *dataFactoryName* i exemplet med lämpliga värden.</span><span class="sxs-lookup"><span data-stu-id="69418-371">Replace *subscriptionId*, *resourceGroupName*, and *dataFactoryName* in the sample with appropriate values.</span></span>

<span data-ttu-id="69418-372">*metricName* stöder för närvarande två värden:</span><span class="sxs-lookup"><span data-stu-id="69418-372">*metricName* currently supports two values:</span></span>

* <span data-ttu-id="69418-373">FailedRuns</span><span class="sxs-lookup"><span data-stu-id="69418-373">FailedRuns</span></span>
* <span data-ttu-id="69418-374">SuccessfulRuns</span><span class="sxs-lookup"><span data-stu-id="69418-374">SuccessfulRuns</span></span>

<span data-ttu-id="69418-375">**Distribuera aviseringen**</span><span class="sxs-lookup"><span data-stu-id="69418-375">**Deploy the alert**</span></span>

<span data-ttu-id="69418-376">Använda Azure PowerShell-cmdlet för att distribuera aviseringen **ny AzureRmResourceGroupDeployment**som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="69418-376">To deploy the alert, use the Azure PowerShell cmdlet **New-AzureRmResourceGroupDeployment**, as shown in the following example:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json
```

<span data-ttu-id="69418-377">Du bör se följande meddelande efter en lyckad distribution:</span><span class="sxs-lookup"><span data-stu-id="69418-377">You should see following message after a successful deployment:</span></span>

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

<span data-ttu-id="69418-378">Du kan också använda den **Lägg till AlertRule** att distribuera en aviseringsregel.</span><span class="sxs-lookup"><span data-stu-id="69418-378">You can also use the **Add-AlertRule** cmdlet to deploy an alert rule.</span></span> <span data-ttu-id="69418-379">Finns det [Lägg till AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) avsnittet information och exempel.</span><span class="sxs-lookup"><span data-stu-id="69418-379">See the [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) topic for details and examples.</span></span>  

## <a name="move-a-data-factory-to-a-different-resource-group-or-subscription"></a><span data-ttu-id="69418-380">Flytta en datafabrik till en annan resursgrupp eller prenumeration</span><span class="sxs-lookup"><span data-stu-id="69418-380">Move a data factory to a different resource group or subscription</span></span>
<span data-ttu-id="69418-381">Du kan flytta en datafabrik till en annan resursgrupp eller en annan prenumeration med hjälp av den **flytta** kommandot liggande knappen på startsidan i din data factory.</span><span class="sxs-lookup"><span data-stu-id="69418-381">You can move a data factory to a different resource group or a different subscription by using the **Move** command bar button on the home page of your data factory.</span></span>

![Flytta data factory](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

<span data-ttu-id="69418-383">Du kan också flytta alla relaterade resurser (till exempel aviseringar som är associerade med data factory), tillsammans med datafabriken.</span><span class="sxs-lookup"><span data-stu-id="69418-383">You can also move any related resources (such as alerts that are associated with the data factory), along with the data factory.</span></span>

![Flytta dialogrutan resurser](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
