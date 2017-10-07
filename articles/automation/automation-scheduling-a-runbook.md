---
title: aaaScheduling en runbook i Azure Automation | Microsoft Docs
description: "Beskriver hur toocreate ett schema i Azure Automation så att automatiskt starta en runbook på en viss tidpunkt eller ett återkommande schema."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 710979ff-99d8-41e4-ac6d-6bf26b8ea654
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/09/2016
ms.author: bwren
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: c215b7ff6aa200466f3be566facba3c0cffcc924
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a><span data-ttu-id="fb734-103">Schemaläggning av en Runbook i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="fb734-103">Scheduling a runbook in Azure Automation</span></span>
<span data-ttu-id="fb734-104">tooschedule en runbook i Azure Automation toostart vid en viss tidpunkt, du länka det tooone eller flera scheman.</span><span class="sxs-lookup"><span data-stu-id="fb734-104">tooschedule a runbook in Azure Automation toostart at a specified time, you link it tooone or more schedules.</span></span> <span data-ttu-id="fb734-105">Ett schema kan vara konfigurerade tooeither köras en gång eller på ett reoccurring timvis eller daglig schema för runbooks i hello klassiska Azure-portalen och runbooks i hello Azure-portalen du kan också schemalägga dem för varje vecka, månad, specifika veckodagar hello eller dagar hello månaden, eller en viss dag i månaden hello.</span><span class="sxs-lookup"><span data-stu-id="fb734-105">A schedule can be configured tooeither run once or on a reoccurring hourly or daily schedule for runbooks in hello Azure classic portal and for runbooks in hello Azure portal,  you can additionally schedule them for weekly, monthly, specific days of hello week or days of hello month, or a particular day of hello month.</span></span>  <span data-ttu-id="fb734-106">En runbook kan vara länkad toomultiple scheman och ett schema kan ha flera runbooks länkade tooit.</span><span class="sxs-lookup"><span data-stu-id="fb734-106">A runbook can be linked toomultiple schedules, and a schedule can have multiple runbooks linked tooit.</span></span>

## <a name="creating-a-schedule"></a><span data-ttu-id="fb734-107">Skapa ett schema</span><span class="sxs-lookup"><span data-stu-id="fb734-107">Creating a schedule</span></span>
<span data-ttu-id="fb734-108">Du kan skapa ett nytt schema för runbooks i hello Azure-portalen i hello klassiska portalen eller med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fb734-108">You can create a new schedule for runbooks in hello Azure portal, in hello classic portal, or with Windows PowerShell.</span></span> <span data-ttu-id="fb734-109">Du kan också ha hello möjlighet att skapa ett nytt schema när du länkar ett schema för tooa av runbook med hjälp av hello Azure klassiska eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fb734-109">You also have hello option of creating a new schedule when you link a runbook tooa schedule using hello Azure classic or Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="fb734-110">När du kopplar ett schema till en runbook Automation lagrar hello aktuella versioner av hello moduler i ditt konto och länkar dem toothat schema.</span><span class="sxs-lookup"><span data-stu-id="fb734-110">When you associate a schedule with a runbook, Automation stores hello current versions of hello modules in your account and links them toothat schedule.</span></span>  <span data-ttu-id="fb734-111">Det innebär att om du hade en modul med version 1.0 i ditt konto när du har skapat ett schema och sedan uppdaterar hello modulen tooversion 2.0 hello schema kommer att fortsätta toouse 1.0.</span><span class="sxs-lookup"><span data-stu-id="fb734-111">This means that if you had a module with version 1.0 in your account when you created a schedule and then update hello module tooversion 2.0, hello schedule will continue toouse 1.0.</span></span>  <span data-ttu-id="fb734-112">I ordning toouse hello uppdaterade Modulversion, måste du skapa ett nytt schema.</span><span class="sxs-lookup"><span data-stu-id="fb734-112">In order toouse hello updated module version, you must create a new schedule.</span></span> 
> 
> 

### <a name="toocreate-a-new-schedule-in-hello-azure-classic-portal"></a><span data-ttu-id="fb734-113">toocreate ett nytt schema i hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fb734-113">toocreate a new schedule in hello Azure classic portal</span></span>
1. <span data-ttu-id="fb734-114">Markera Automation i hello klassiska Azure-portalen, och välj sedan hello namnet på ett automation-konto.</span><span class="sxs-lookup"><span data-stu-id="fb734-114">In hello Azure classic portal, select Automation and then then select hello name of an automation account.</span></span>
2. <span data-ttu-id="fb734-115">Välj hello **tillgångar** fliken.</span><span class="sxs-lookup"><span data-stu-id="fb734-115">Select hello **Assets** tab.</span></span>
3. <span data-ttu-id="fb734-116">Hello längst ned i hello-fönstret klickar du på **Lägg till inställning**.</span><span class="sxs-lookup"><span data-stu-id="fb734-116">At hello bottom of hello window, click **Add Setting**.</span></span>
4. <span data-ttu-id="fb734-117">Klicka på **Lägg till schema**.</span><span class="sxs-lookup"><span data-stu-id="fb734-117">Click **Add Schedule**.</span></span>
5. <span data-ttu-id="fb734-118">Ange en **namn** och eventuellt en **beskrivning** för hello nya schedule.your schemat ska köras **en gång**, **timvis**, **Dagliga**, **veckovisa**, eller **månatliga**.</span><span class="sxs-lookup"><span data-stu-id="fb734-118">Type a **Name** and optionally a **Description** for hello new schedule.your schedule will run **One Time**, **Hourly**, **Daily**, **Weekly**, or **Monthly**.</span></span>
6. <span data-ttu-id="fb734-119">Ange en **starttid** och andra alternativ beroende på hello typ av schema som du har valt.</span><span class="sxs-lookup"><span data-stu-id="fb734-119">Specify a **Start Time** and other options depending on hello type of schedule that you selected.</span></span>

### <a name="toocreate-a-new-schedule-in-hello-azure-portal"></a><span data-ttu-id="fb734-120">toocreate ett nytt schema i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fb734-120">toocreate a new schedule in hello Azure portal</span></span>
1. <span data-ttu-id="fb734-121">I hello Azure-portalen från ditt automation-konto klickar du på hello **tillgångar** panelen tooopen hello **tillgångar** bladet.</span><span class="sxs-lookup"><span data-stu-id="fb734-121">In hello Azure portal, from your automation account, click hello **Assets** tile tooopen hello **Assets** blade.</span></span>
2. <span data-ttu-id="fb734-122">Klicka på hello **scheman** panelen tooopen hello **scheman** bladet.</span><span class="sxs-lookup"><span data-stu-id="fb734-122">Click hello **Schedules** tile tooopen hello **Schedules** blade.</span></span>
3. <span data-ttu-id="fb734-123">Klicka på **lägga till ett schema** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="fb734-123">Click **Add a schedule** at hello top of hello blade.</span></span>
4. <span data-ttu-id="fb734-124">På hello **nytt schema** bladet typ a **namn** och eventuellt en **beskrivning** för hello nytt schema.</span><span class="sxs-lookup"><span data-stu-id="fb734-124">On hello **New schedule** blade, type a **Name** and optionally a **Description** for hello new schedule.</span></span>
5. <span data-ttu-id="fb734-125">Välj om hello schemat ska köras en gång, eller på ett reoccurring schema genom att välja **när** eller **återkommande**.</span><span class="sxs-lookup"><span data-stu-id="fb734-125">Select whether hello schedule will run one time, or on a reoccurring schedule by selecting **Once** or **Recurrence**.</span></span>  <span data-ttu-id="fb734-126">Om du väljer **när** ange en **starttid** och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="fb734-126">If you select **Once** specify a **Start time** and then click **Create**.</span></span>  <span data-ttu-id="fb734-127">Om du väljer **återkommande**, ange en **starttid** och hello frekvens för hur ofta du vill hello runbook toorepeat - av **timme**, **dag**, **vecka**, eller av **månad**.</span><span class="sxs-lookup"><span data-stu-id="fb734-127">If you select **Recurrence**, specify a **Start time** and hello frequency for how often you want hello runbook toorepeat - by **hour**, **day**, **week**, or by **month**.</span></span>  <span data-ttu-id="fb734-128">Om du väljer **vecka** eller **månad** hello nedrullningsbara listan hello **upprepning alternativet** visas i bladet hello och vid val av hello **upprepning alternativet** bladet visas och du kan välja hello veckodag om du har valt **vecka**.</span><span class="sxs-lookup"><span data-stu-id="fb734-128">If you select **week** or **month** from hello drop-down list, hello **Recurrence option** will appear in hello blade and upon selection, hello **Recurrence option** blade will be presented and you can select hello day of week if you selected **week**.</span></span>  <span data-ttu-id="fb734-129">Om du har valt **månad**, du kan välja efter **veckodagar** eller särskilda dagar i månaden hello på hello kalender och slutligen vill du toorun på hello sista dagen i månaden hello eller inte och klicka sedan på **OK** .</span><span class="sxs-lookup"><span data-stu-id="fb734-129">If you selected **month**, you can choose by **week days** or specific days of hello month on hello calendar and finally, do you want toorun it on hello last day of hello month or not and then click **OK**.</span></span>   

### <a name="toocreate-a-new-schedule-with-windows-powershell"></a><span data-ttu-id="fb734-130">toocreate ett nytt schema med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="fb734-130">toocreate a new schedule with Windows PowerShell</span></span>
<span data-ttu-id="fb734-131">Du kan använda hello [ny AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet toocreate ett nytt schema i Azure Automation för klassiska runbooks eller [ny AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet för runbooks i hello Azure portalen.</span><span class="sxs-lookup"><span data-stu-id="fb734-131">You can use hello [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet toocreate a new schedule in Azure Automation for classic runbooks, or [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet for runbooks in hello Azure portal.</span></span> <span data-ttu-id="fb734-132">Du måste ange hello starttiden för hello schema och hello frekvens som ska köras.</span><span class="sxs-lookup"><span data-stu-id="fb734-132">You must specify hello start time for hello schedule and hello frequency it should run.</span></span>

<span data-ttu-id="fb734-133">hello följande exempelkommandon visar hur toocreate ett nytt schema som körs varje dag klockan 3:30 startar på 20 januari 2015 med Azure Service Management-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="fb734-133">hello following sample commands show how toocreate a new schedule that runs each day at 3:30 PM starting on January 20, 2015 with an Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

<span data-ttu-id="fb734-134">hello följande exempel kommandon visar hur toocreate ett schema för hello 15 och 30: e i månaden med en Azure Resource Manager-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="fb734-134">hello following sample commands shows how toocreate a schedule for hello 15th and 30th of every month using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"


## <a name="linking-a-schedule-tooa-runbook"></a><span data-ttu-id="fb734-135">Länka ett schema tooa runbook</span><span class="sxs-lookup"><span data-stu-id="fb734-135">Linking a schedule tooa runbook</span></span>
<span data-ttu-id="fb734-136">En runbook kan vara länkad toomultiple scheman och ett schema kan ha flera runbooks länkade tooit.</span><span class="sxs-lookup"><span data-stu-id="fb734-136">A runbook can be linked toomultiple schedules, and a schedule can have multiple runbooks linked tooit.</span></span> <span data-ttu-id="fb734-137">Om en runbook har parametrar, kan du ange värden för dessa.</span><span class="sxs-lookup"><span data-stu-id="fb734-137">If a runbook has parameters, then you can provide values for them.</span></span> <span data-ttu-id="fb734-138">Du måste ange värden för alla obligatoriska parametrar och kan ange värden för valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="fb734-138">You must provide values for any mandatory parameters and may provide values for any optional parameters.</span></span>  <span data-ttu-id="fb734-139">Dessa värden kommer att användas varje gång hello runbook har startats med det här schemat.</span><span class="sxs-lookup"><span data-stu-id="fb734-139">These values will be used each time hello runbook is started by this schedule.</span></span>  <span data-ttu-id="fb734-140">Du kan koppla hello samma runbook tooanother schema och ange olika parametervärden.</span><span class="sxs-lookup"><span data-stu-id="fb734-140">You can attach hello same runbook tooanother schedule and specify different parameter values.</span></span>

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-classic-portal"></a><span data-ttu-id="fb734-141">toolink en schema tooa runbook med hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fb734-141">toolink a schedule tooa runbook with hello Azure classic portal</span></span>
1. <span data-ttu-id="fb734-142">Välj i hello klassiska Azure-portalen, **Automation** och klicka sedan på hello namnet på ett automation-konto.</span><span class="sxs-lookup"><span data-stu-id="fb734-142">In hello Azure classic portal, select **Automation** and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="fb734-143">Välj hello **Runbooks** fliken.</span><span class="sxs-lookup"><span data-stu-id="fb734-143">Select hello **Runbooks** tab.</span></span>
3. <span data-ttu-id="fb734-144">Klicka på hello namnet på hello runbook tooschedule.</span><span class="sxs-lookup"><span data-stu-id="fb734-144">Click on hello name of hello runbook tooschedule.</span></span>
4. <span data-ttu-id="fb734-145">Klicka på hello **schema** fliken.</span><span class="sxs-lookup"><span data-stu-id="fb734-145">Click hello **Schedule** tab.</span></span>
5. <span data-ttu-id="fb734-146">Om hello runbook inte är för närvarande länkade tooa schema, så får du hello alternativet för**länka tooa nytt schema** eller **länka tooan befintliga schema**.</span><span class="sxs-lookup"><span data-stu-id="fb734-146">If hello runbook is not currently linked tooa schedule, then you will be given hello option too**Link tooa New Schedule** or **Link tooan Existing Schedule**.</span></span>  <span data-ttu-id="fb734-147">Om hello runbook är för närvarande länkade tooa schema, klickar du på **länk** på hello längst ned på hello fönstret tooaccess dessa alternativ.</span><span class="sxs-lookup"><span data-stu-id="fb734-147">If hello runbook is currently linked tooa schedule, click **Link** at hello bottom of hello window tooaccess these options.</span></span>
6. <span data-ttu-id="fb734-148">Om hello runbook har parametrar uppmanas för deras värden.</span><span class="sxs-lookup"><span data-stu-id="fb734-148">If hello runbook has parameters, you will be prompted for their values.</span></span>  

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-portal"></a><span data-ttu-id="fb734-149">toolink en schema tooa runbook med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fb734-149">toolink a schedule tooa runbook with hello Azure portal</span></span>
1. <span data-ttu-id="fb734-150">I hello Azure-portalen från ditt automation-konto klickar du på hello **Runbooks** panelen tooopen hello **Runbooks** bladet.</span><span class="sxs-lookup"><span data-stu-id="fb734-150">In hello Azure portal, from your automation account, click hello **Runbooks** tile tooopen hello **Runbooks** blade.</span></span>
2. <span data-ttu-id="fb734-151">Klicka på hello namnet på hello runbook tooschedule.</span><span class="sxs-lookup"><span data-stu-id="fb734-151">Click on hello name of hello runbook tooschedule.</span></span>
3. <span data-ttu-id="fb734-152">Om hello runbook inte är för närvarande länkade tooa schema, kommer du att angivna hello alternativet toocreate ett nytt schema eller länka tooan befintliga schema.</span><span class="sxs-lookup"><span data-stu-id="fb734-152">If hello runbook is not currently linked tooa schedule, then you will be given hello option toocreate a new schedule or link tooan existing schedule.</span></span>  
4. <span data-ttu-id="fb734-153">Om hello runbook har parametrar, kan du välja alternativet hello **ändra körningsinställningar (standard: Azure)** och hello **parametrar** bladet visas där du kan ange hello information i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="fb734-153">If hello runbook has parameters, you can select hello option **Modify run settings (Default:Azure)** and hello **Parameters** blade is presented where you can enter hello information accordingly.</span></span>  

### <a name="toolink-a-schedule-tooa-runbook-with-windows-powershell"></a><span data-ttu-id="fb734-154">toolink ett schema tooa runbook med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="fb734-154">toolink a schedule tooa runbook with Windows PowerShell</span></span>
<span data-ttu-id="fb734-155">Du kan använda hello [registrera AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) toolink runbook ett schema tooa klassiska eller [registrera AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet för runbooks i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fb734-155">You can use hello [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) toolink a schedule tooa classic runbook or [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet for runbooks in hello Azure portal.</span></span>  <span data-ttu-id="fb734-156">Du kan ange värden för hello runbook-parametrar med hello parametrar parameter.</span><span class="sxs-lookup"><span data-stu-id="fb734-156">You can specify values for hello runbook’s parameters with hello Parameters parameter.</span></span> <span data-ttu-id="fb734-157">Se [starta en Runbook i Azure Automation](automation-starting-a-runbook.md) mer information om specificering av parametervärden.</span><span class="sxs-lookup"><span data-stu-id="fb734-157">See [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) for more information on specifying parameter values.</span></span>

<span data-ttu-id="fb734-158">hello följande exempel kommandon visar hur toolink ett schema med en Azure Service Management-cmdlet med parametrar.</span><span class="sxs-lookup"><span data-stu-id="fb734-158">hello following sample commands show how toolink a schedule using an Azure Service Management cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

<span data-ttu-id="fb734-159">hello följande exempel kommandon visar hur toolink en schema tooa runbook med en Azure Resource Manager-cmdlet med parametrar.</span><span class="sxs-lookup"><span data-stu-id="fb734-159">hello following sample commands show how toolink a schedule tooa runbook using an Azure Resource Manager cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a><span data-ttu-id="fb734-160">Inaktivera ett schema</span><span class="sxs-lookup"><span data-stu-id="fb734-160">Disabling a schedule</span></span>
<span data-ttu-id="fb734-161">När du inaktiverar ett schema körs inte längre alla runbooks som är länkade tooit på schemat.</span><span class="sxs-lookup"><span data-stu-id="fb734-161">When you disable a schedule, any runbooks linked tooit will no longer run on that schedule.</span></span> <span data-ttu-id="fb734-162">Du kan manuellt inaktivera ett schema eller ange en förfallotid för scheman med en frekvens när de skapas.</span><span class="sxs-lookup"><span data-stu-id="fb734-162">You can manually disable a schedule or set an expiration time for schedules with a frequency when you create them.</span></span> <span data-ttu-id="fb734-163">När hello giltighetstid har uppnåtts, ska hello schemat inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="fb734-163">When hello expiration time is reached, hello schedule will be disabled.</span></span>

### <a name="toodisable-a-schedule-from-hello-azure-classic-portal"></a><span data-ttu-id="fb734-164">toodisable ett schema från hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fb734-164">toodisable a schedule from hello Azure classic portal</span></span>
<span data-ttu-id="fb734-165">Du kan inaktivera ett schema i hello klassiska Azure-portalen från hello Schemadetaljer sida för hello schema.</span><span class="sxs-lookup"><span data-stu-id="fb734-165">You can disable a schedule in hello Azure classic portal from hello Schedule Details page for hello schedule.</span></span>

1. <span data-ttu-id="fb734-166">Välj Automation i hello klassiska Azure-portalen, och klicka sedan på hello namnet på ett automation-konto.</span><span class="sxs-lookup"><span data-stu-id="fb734-166">In hello Azure classic portal, select Automation and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="fb734-167">Fliken hello tillgångar.</span><span class="sxs-lookup"><span data-stu-id="fb734-167">Select hello Assets tab.</span></span>
3. <span data-ttu-id="fb734-168">Klicka på ett schema tooopen hello namn dess detaljsida.</span><span class="sxs-lookup"><span data-stu-id="fb734-168">Click hello name of a schedule tooopen its detail page.</span></span>
4. <span data-ttu-id="fb734-169">Ändra **aktiverad** för**nr**.</span><span class="sxs-lookup"><span data-stu-id="fb734-169">Change **Enabled** too**No**.</span></span>

### <a name="toodisable-a-schedule-from-hello-azure-portal"></a><span data-ttu-id="fb734-170">toodisable ett schema från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fb734-170">toodisable a schedule from hello Azure portal</span></span>
1. <span data-ttu-id="fb734-171">I hello Azure-portalen från ditt automation-konto klickar du på hello **tillgångar** panelen tooopen hello **tillgångar** bladet.</span><span class="sxs-lookup"><span data-stu-id="fb734-171">In hello Azure portal, from your automation account, click hello **Assets** tile tooopen hello **Assets** blade.</span></span>
2. <span data-ttu-id="fb734-172">Klicka på hello **scheman** panelen tooopen hello **scheman** bladet.</span><span class="sxs-lookup"><span data-stu-id="fb734-172">Click hello **Schedules** tile tooopen hello **Schedules** blade.</span></span>
3. <span data-ttu-id="fb734-173">Klicka på ett schema tooopen hello informationsbladet hello namn.</span><span class="sxs-lookup"><span data-stu-id="fb734-173">Click hello name of a schedule tooopen hello details blade.</span></span>
4. <span data-ttu-id="fb734-174">Ändra **aktiverad** för**nr**.</span><span class="sxs-lookup"><span data-stu-id="fb734-174">Change **Enabled** too**No**.</span></span>

### <a name="toodisable-a-schedule-with-windows-powershell"></a><span data-ttu-id="fb734-175">toodisable ett schema med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="fb734-175">toodisable a schedule with Windows PowerShell</span></span>
<span data-ttu-id="fb734-176">Du kan använda hello [Set AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet toochange hello egenskaper för ett befintligt schema för en klassiska runbook eller [Set AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet för runbooks i hello Azure portalen.</span><span class="sxs-lookup"><span data-stu-id="fb734-176">You can use hello [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet toochange hello properties of an existing schedule for a classic runbook or [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet for runbooks in hello Azure portal.</span></span> <span data-ttu-id="fb734-177">toodisable hello schema, ange **FALSKT** för hello **IsEnabled** parameter.</span><span class="sxs-lookup"><span data-stu-id="fb734-177">toodisable hello schedule, specify **false** for hello **IsEnabled** parameter.</span></span>

<span data-ttu-id="fb734-178">hello följande exempelkommandon visar hur toodisable ett schema med hello Azure Service Management-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="fb734-178">hello following sample commands show how toodisable a schedule using hello Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

<span data-ttu-id="fb734-179">hello följande exempel kommandon visar hur toodisable ett schema för en runbook med en Azure Resource Manager-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="fb734-179">hello following sample commands show how toodisable a schedule for a runbook using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a><span data-ttu-id="fb734-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb734-180">Next steps</span></span>
* <span data-ttu-id="fb734-181">toolearn mer information om hur du arbetar med scheman, se [schema tillgångar i Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span><span class="sxs-lookup"><span data-stu-id="fb734-181">toolearn more about working with schedules, see [Schedule Assets in Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span></span>
* <span data-ttu-id="fb734-182">tooget igång med runbooks i Azure Automation finns [starta en Runbook i Azure Automation](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="fb734-182">tooget started with runbooks in Azure Automation, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md)</span></span> 

