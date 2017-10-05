---
title: "Schemaläggning av en runbook i Azure Automation | Microsoft Docs"
description: "Beskriver hur du skapar ett schema i Azure Automation så att automatiskt starta en runbook på en viss tidpunkt eller ett återkommande schema."
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
ms.openlocfilehash: 52f1d55f141bb1b3948e3b7039cfc131a5e407b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a><span data-ttu-id="887a8-103">Schemaläggning av en Runbook i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="887a8-103">Scheduling a runbook in Azure Automation</span></span>
<span data-ttu-id="887a8-104">Om du vill schemalägga en runbook i Azure Automation för att starta vid en viss tidpunkt länka du det till ett eller flera scheman.</span><span class="sxs-lookup"><span data-stu-id="887a8-104">To schedule a runbook in Azure Automation to start at a specified time, you link it to one or more schedules.</span></span> <span data-ttu-id="887a8-105">Ett schema kan konfigureras för att köras en gång eller på en igen timvis eller daglig schema för runbooks i den klassiska Azure-portalen och runbooks i Azure-portalen, kan du dessutom schemalägga dem för varje vecka, månad, särskilda dagar i veckan eller dagar efter m h eller en viss dag i månaden.</span><span class="sxs-lookup"><span data-stu-id="887a8-105">A schedule can be configured to either run once or on a reoccurring hourly or daily schedule for runbooks in the Azure classic portal and for runbooks in the Azure portal,  you can additionally schedule them for weekly, monthly, specific days of the week or days of the month, or a particular day of the month.</span></span>  <span data-ttu-id="887a8-106">En runbook kan länkas till flera scheman och ett schema kan ha flera runbooks som är länkade till den.</span><span class="sxs-lookup"><span data-stu-id="887a8-106">A runbook can be linked to multiple schedules, and a schedule can have multiple runbooks linked to it.</span></span>

## <a name="creating-a-schedule"></a><span data-ttu-id="887a8-107">Skapa ett schema</span><span class="sxs-lookup"><span data-stu-id="887a8-107">Creating a schedule</span></span>
<span data-ttu-id="887a8-108">Du kan skapa ett nytt schema för runbooks i Azure-portalen i den klassiska portalen eller med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="887a8-108">You can create a new schedule for runbooks in the Azure portal, in the classic portal, or with Windows PowerShell.</span></span> <span data-ttu-id="887a8-109">Du har också möjlighet att skapa ett nytt schema när du länkar en runbook till ett schema med Azure klassiska eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="887a8-109">You also have the option of creating a new schedule when you link a runbook to a schedule using the Azure classic or Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="887a8-110">När du kopplar ett schema till en runbook Automation lagrar aktuella versioner av moduler i ditt konto och kopplar dem till schemat.</span><span class="sxs-lookup"><span data-stu-id="887a8-110">When you associate a schedule with a runbook, Automation stores the current versions of the modules in your account and links them to that schedule.</span></span>  <span data-ttu-id="887a8-111">Det innebär att om du har en modul med version 1.0 i ditt konto när du har skapat ett schema och uppdatera modulen till version 2.0, schemat kommer att fortsätta att använda 1.0.</span><span class="sxs-lookup"><span data-stu-id="887a8-111">This means that if you had a module with version 1.0 in your account when you created a schedule and then update the module to version 2.0, the schedule will continue to use 1.0.</span></span>  <span data-ttu-id="887a8-112">För att kunna använda den uppdaterade modulversionen, måste du skapa ett nytt schema.</span><span class="sxs-lookup"><span data-stu-id="887a8-112">In order to use the updated module version, you must create a new schedule.</span></span> 
> 
> 

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a><span data-ttu-id="887a8-113">Skapa ett nytt schema i den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="887a8-113">To create a new schedule in the Azure classic portal</span></span>
1. <span data-ttu-id="887a8-114">Markera Automation och välj sedan namnet på ett automation-konto i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="887a8-114">In the Azure classic portal, select Automation and then then select the name of an automation account.</span></span>
2. <span data-ttu-id="887a8-115">Välj den **tillgångar** fliken.</span><span class="sxs-lookup"><span data-stu-id="887a8-115">Select the **Assets** tab.</span></span>
3. <span data-ttu-id="887a8-116">Längst ned i fönstret klickar du på **Lägg till inställning**.</span><span class="sxs-lookup"><span data-stu-id="887a8-116">At the bottom of the window, click **Add Setting**.</span></span>
4. <span data-ttu-id="887a8-117">Klicka på **Lägg till schema**.</span><span class="sxs-lookup"><span data-stu-id="887a8-117">Click **Add Schedule**.</span></span>
5. <span data-ttu-id="887a8-118">Ange en **namn** och eventuellt en **beskrivning** för nya schedule.your schemat ska köras **en gång**, **timvis**, **dagliga**, **veckovisa**, eller **månatliga**.</span><span class="sxs-lookup"><span data-stu-id="887a8-118">Type a **Name** and optionally a **Description** for the new schedule.your schedule will run **One Time**, **Hourly**, **Daily**, **Weekly**, or **Monthly**.</span></span>
6. <span data-ttu-id="887a8-119">Ange en **starttid** och andra alternativ beroende på vilken typ av schema som du har valt.</span><span class="sxs-lookup"><span data-stu-id="887a8-119">Specify a **Start Time** and other options depending on the type of schedule that you selected.</span></span>

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a><span data-ttu-id="887a8-120">Skapa ett nytt schema i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="887a8-120">To create a new schedule in the Azure portal</span></span>
1. <span data-ttu-id="887a8-121">I Azure-portalen från ditt automation-konto klickar du på den **tillgångar** öppna den **tillgångar** bladet.</span><span class="sxs-lookup"><span data-stu-id="887a8-121">In the Azure portal, from your automation account, click the **Assets** tile to open the **Assets** blade.</span></span>
2. <span data-ttu-id="887a8-122">Klicka på den **scheman** öppna den **scheman** bladet.</span><span class="sxs-lookup"><span data-stu-id="887a8-122">Click the **Schedules** tile to open the **Schedules** blade.</span></span>
3. <span data-ttu-id="887a8-123">Klicka på **lägga till ett schema** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="887a8-123">Click **Add a schedule** at the top of the blade.</span></span>
4. <span data-ttu-id="887a8-124">På den **nytt schema** bladet typ a **namn** och eventuellt en **beskrivning** för det nya schemat.</span><span class="sxs-lookup"><span data-stu-id="887a8-124">On the **New schedule** blade, type a **Name** and optionally a **Description** for the new schedule.</span></span>
5. <span data-ttu-id="887a8-125">Välj om schemat ska köras en gång, eller på ett reoccurring schema genom att välja **när** eller **återkommande**.</span><span class="sxs-lookup"><span data-stu-id="887a8-125">Select whether the schedule will run one time, or on a reoccurring schedule by selecting **Once** or **Recurrence**.</span></span>  <span data-ttu-id="887a8-126">Om du väljer **när** ange en **starttid** och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="887a8-126">If you select **Once** specify a **Start time** and then click **Create**.</span></span>  <span data-ttu-id="887a8-127">Om du väljer **återkommande**, ange en **starttid** och frekvens för hur ofta du vill att runbook ska upprepas - av **timme**, **dag**, **vecka**, eller av **månad**.</span><span class="sxs-lookup"><span data-stu-id="887a8-127">If you select **Recurrence**, specify a **Start time** and the frequency for how often you want the runbook to repeat - by **hour**, **day**, **week**, or by **month**.</span></span>  <span data-ttu-id="887a8-128">Om du väljer **vecka** eller **månad** från den nedrullningsbara listan den **upprepning alternativet** visas i bladet och vid val av den **upprepning alternativet** bladet visas och du kan välja dag i veckan om du har valt **vecka**.</span><span class="sxs-lookup"><span data-stu-id="887a8-128">If you select **week** or **month** from the drop-down list, the **Recurrence option** will appear in the blade and upon selection, the **Recurrence option** blade will be presented and you can select the day of week if you selected **week**.</span></span>  <span data-ttu-id="887a8-129">Om du har valt **månad**, du kan välja efter **veckodagar** eller särskilda dagar i månaden i kalendern och slutligen vill du köra den på den sista dagen i månaden eller inte och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="887a8-129">If you selected **month**, you can choose by **week days** or specific days of the month on the calendar and finally, do you want to run it on the last day of the month or not and then click **OK**.</span></span>   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a><span data-ttu-id="887a8-130">Skapa ett nytt schema med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="887a8-130">To create a new schedule with Windows PowerShell</span></span>
<span data-ttu-id="887a8-131">Du kan använda den [ny AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) för att skapa ett nytt schema i Azure Automation för klassiska runbooks eller [ny AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet för runbooks i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="887a8-131">You can use the [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet to create a new schedule in Azure Automation for classic runbooks, or [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet for runbooks in the Azure portal.</span></span> <span data-ttu-id="887a8-132">Du måste ange starttiden för schemat och frekvensen ska köras.</span><span class="sxs-lookup"><span data-stu-id="887a8-132">You must specify the start time for the schedule and the frequency it should run.</span></span>

<span data-ttu-id="887a8-133">Följande exempelkommandon visar hur du skapar ett nytt schema som körs varje dag klockan 3:30 startar på 20 januari 2015 med Azure Service Management-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="887a8-133">The following sample commands show how to create a new schedule that runs each day at 3:30 PM starting on January 20, 2015 with an Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

<span data-ttu-id="887a8-134">Följande exempelkommandon visar hur du skapar ett schema för den 15: e och 30: e i månaden med en Azure Resource Manager-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="887a8-134">The following sample commands shows how to create a schedule for the 15th and 30th of every month using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"


## <a name="linking-a-schedule-to-a-runbook"></a><span data-ttu-id="887a8-135">Länka ett schema till en runbook</span><span class="sxs-lookup"><span data-stu-id="887a8-135">Linking a schedule to a runbook</span></span>
<span data-ttu-id="887a8-136">En runbook kan länkas till flera scheman och ett schema kan ha flera runbooks som är länkade till den.</span><span class="sxs-lookup"><span data-stu-id="887a8-136">A runbook can be linked to multiple schedules, and a schedule can have multiple runbooks linked to it.</span></span> <span data-ttu-id="887a8-137">Om en runbook har parametrar, kan du ange värden för dessa.</span><span class="sxs-lookup"><span data-stu-id="887a8-137">If a runbook has parameters, then you can provide values for them.</span></span> <span data-ttu-id="887a8-138">Du måste ange värden för alla obligatoriska parametrar och kan ange värden för valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="887a8-138">You must provide values for any mandatory parameters and may provide values for any optional parameters.</span></span>  <span data-ttu-id="887a8-139">Dessa värden används varje gång runbook startas med det här schemat.</span><span class="sxs-lookup"><span data-stu-id="887a8-139">These values will be used each time the runbook is started by this schedule.</span></span>  <span data-ttu-id="887a8-140">Du kan koppla samma runbook till ett annat schema och ange olika parametervärden.</span><span class="sxs-lookup"><span data-stu-id="887a8-140">You can attach the same runbook to another schedule and specify different parameter values.</span></span>

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a><span data-ttu-id="887a8-141">Länka ett schema till en runbook med den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="887a8-141">To link a schedule to a runbook with the Azure classic portal</span></span>
1. <span data-ttu-id="887a8-142">I den klassiska Azure-portalen väljer **Automation** och klicka sedan på namnet på ett automation-konto.</span><span class="sxs-lookup"><span data-stu-id="887a8-142">In the Azure classic portal, select **Automation** and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="887a8-143">Välj den **Runbooks** fliken.</span><span class="sxs-lookup"><span data-stu-id="887a8-143">Select the **Runbooks** tab.</span></span>
3. <span data-ttu-id="887a8-144">Klicka på namnet på runbooken som ska schemaläggas.</span><span class="sxs-lookup"><span data-stu-id="887a8-144">Click on the name of the runbook to schedule.</span></span>
4. <span data-ttu-id="887a8-145">Klicka på den **schema** fliken.</span><span class="sxs-lookup"><span data-stu-id="887a8-145">Click the **Schedule** tab.</span></span>
5. <span data-ttu-id="887a8-146">Om runbook inte är för närvarande kopplad till ett schema, så får du alternativet att **länk till ett nytt schema** eller **länk till ett befintligt schema**.</span><span class="sxs-lookup"><span data-stu-id="887a8-146">If the runbook is not currently linked to a schedule, then you will be given the option to **Link to a New Schedule** or **Link to an Existing Schedule**.</span></span>  <span data-ttu-id="887a8-147">Om runbooken för närvarande är länkad till ett schema, klickar du på **länk** längst ned i fönstret för att komma åt dessa alternativ.</span><span class="sxs-lookup"><span data-stu-id="887a8-147">If the runbook is currently linked to a schedule, click **Link** at the bottom of the window to access these options.</span></span>
6. <span data-ttu-id="887a8-148">Om runbooken har parametrar, att du uppmanas deras värden.</span><span class="sxs-lookup"><span data-stu-id="887a8-148">If the runbook has parameters, you will be prompted for their values.</span></span>  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a><span data-ttu-id="887a8-149">Länka ett schema till en runbook med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="887a8-149">To link a schedule to a runbook with the Azure portal</span></span>
1. <span data-ttu-id="887a8-150">I Azure-portalen från ditt automation-konto klickar du på den **Runbooks** öppna den **Runbooks** bladet.</span><span class="sxs-lookup"><span data-stu-id="887a8-150">In the Azure portal, from your automation account, click the **Runbooks** tile to open the **Runbooks** blade.</span></span>
2. <span data-ttu-id="887a8-151">Klicka på namnet på runbooken som ska schemaläggas.</span><span class="sxs-lookup"><span data-stu-id="887a8-151">Click on the name of the runbook to schedule.</span></span>
3. <span data-ttu-id="887a8-152">Om runbook inte är för närvarande kopplad till ett schema, sedan ges du möjlighet att skapa ett nytt schema eller länka till ett befintligt schema.</span><span class="sxs-lookup"><span data-stu-id="887a8-152">If the runbook is not currently linked to a schedule, then you will be given the option to create a new schedule or link to an existing schedule.</span></span>  
4. <span data-ttu-id="887a8-153">Om runbooken har parametrar, kan du välja alternativet **ändra körningsinställningar (standard: Azure)** och **parametrar** bladet visas där du kan ange information i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="887a8-153">If the runbook has parameters, you can select the option **Modify run settings (Default:Azure)** and the **Parameters** blade is presented where you can enter the information accordingly.</span></span>  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a><span data-ttu-id="887a8-154">Länka ett schema till en runbook med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="887a8-154">To link a schedule to a runbook with Windows PowerShell</span></span>
<span data-ttu-id="887a8-155">Du kan använda den [registrera AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) länka ett schema till en klassisk runbook eller [registrera AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet för runbooks i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="887a8-155">You can use the [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) to link a schedule to a classic runbook or [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet for runbooks in the Azure portal.</span></span>  <span data-ttu-id="887a8-156">Du kan ange värden för runbookens parametrar med parametern parametrar.</span><span class="sxs-lookup"><span data-stu-id="887a8-156">You can specify values for the runbook’s parameters with the Parameters parameter.</span></span> <span data-ttu-id="887a8-157">Se [starta en Runbook i Azure Automation](automation-starting-a-runbook.md) mer information om specificering av parametervärden.</span><span class="sxs-lookup"><span data-stu-id="887a8-157">See [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) for more information on specifying parameter values.</span></span>

<span data-ttu-id="887a8-158">Följande exempelkommandon visar hur du länkar ett schema med en Azure Service Management-cmdlet med parametrar.</span><span class="sxs-lookup"><span data-stu-id="887a8-158">The following sample commands show how to link a schedule using an Azure Service Management cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

<span data-ttu-id="887a8-159">Följande exempelkommandon visar hur du länkar ett schema till en runbook med en Azure Resource Manager-cmdlet med parametrar.</span><span class="sxs-lookup"><span data-stu-id="887a8-159">The following sample commands show how to link a schedule to a runbook using an Azure Resource Manager cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a><span data-ttu-id="887a8-160">Inaktivera ett schema</span><span class="sxs-lookup"><span data-stu-id="887a8-160">Disabling a schedule</span></span>
<span data-ttu-id="887a8-161">När du inaktiverar ett schema körs inte längre alla runbooks som är kopplade till schemat.</span><span class="sxs-lookup"><span data-stu-id="887a8-161">When you disable a schedule, any runbooks linked to it will no longer run on that schedule.</span></span> <span data-ttu-id="887a8-162">Du kan manuellt inaktivera ett schema eller ange en förfallotid för scheman med en frekvens när de skapas.</span><span class="sxs-lookup"><span data-stu-id="887a8-162">You can manually disable a schedule or set an expiration time for schedules with a frequency when you create them.</span></span> <span data-ttu-id="887a8-163">Schemat kommer att inaktiveras när förfallotid har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="887a8-163">When the expiration time is reached, the schedule will be disabled.</span></span>

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a><span data-ttu-id="887a8-164">Inaktivera ett schema från den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="887a8-164">To disable a schedule from the Azure classic portal</span></span>
<span data-ttu-id="887a8-165">Du kan inaktivera ett schema i den klassiska Azure-portalen från sidan Schemadetaljer för schemat.</span><span class="sxs-lookup"><span data-stu-id="887a8-165">You can disable a schedule in the Azure classic portal from the Schedule Details page for the schedule.</span></span>

1. <span data-ttu-id="887a8-166">Välj Automation och klicka sedan på namnet på ett automation-konto i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="887a8-166">In the Azure classic portal, select Automation and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="887a8-167">Välj fliken tillgångar.</span><span class="sxs-lookup"><span data-stu-id="887a8-167">Select the Assets tab.</span></span>
3. <span data-ttu-id="887a8-168">Klicka på namnet på ett schema för att öppna dess detaljsida.</span><span class="sxs-lookup"><span data-stu-id="887a8-168">Click the name of a schedule to open its detail page.</span></span>
4. <span data-ttu-id="887a8-169">Ändra **aktiverat** till **nr**.</span><span class="sxs-lookup"><span data-stu-id="887a8-169">Change **Enabled** to **No**.</span></span>

### <a name="to-disable-a-schedule-from-the-azure-portal"></a><span data-ttu-id="887a8-170">Inaktivera ett schema från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="887a8-170">To disable a schedule from the Azure portal</span></span>
1. <span data-ttu-id="887a8-171">I Azure-portalen från ditt automation-konto klickar du på den **tillgångar** öppna den **tillgångar** bladet.</span><span class="sxs-lookup"><span data-stu-id="887a8-171">In the Azure portal, from your automation account, click the **Assets** tile to open the **Assets** blade.</span></span>
2. <span data-ttu-id="887a8-172">Klicka på den **scheman** öppna den **scheman** bladet.</span><span class="sxs-lookup"><span data-stu-id="887a8-172">Click the **Schedules** tile to open the **Schedules** blade.</span></span>
3. <span data-ttu-id="887a8-173">Klicka på namnet på ett schema för att öppna informationsbladet.</span><span class="sxs-lookup"><span data-stu-id="887a8-173">Click the name of a schedule to open the details blade.</span></span>
4. <span data-ttu-id="887a8-174">Ändra **aktiverat** till **nr**.</span><span class="sxs-lookup"><span data-stu-id="887a8-174">Change **Enabled** to **No**.</span></span>

### <a name="to-disable-a-schedule-with-windows-powershell"></a><span data-ttu-id="887a8-175">Inaktivera ett schema med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="887a8-175">To disable a schedule with Windows PowerShell</span></span>
<span data-ttu-id="887a8-176">Du kan använda den [Set AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) för att ändra egenskaperna för ett befintligt schema för en klassiska runbook eller [Set AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet för runbooks i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="887a8-176">You can use the [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet to change the properties of an existing schedule for a classic runbook or [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet for runbooks in the Azure portal.</span></span> <span data-ttu-id="887a8-177">Om du vill inaktivera schemat, ange **FALSKT** för den **IsEnabled** parameter.</span><span class="sxs-lookup"><span data-stu-id="887a8-177">To disable the schedule, specify **false** for the **IsEnabled** parameter.</span></span>

<span data-ttu-id="887a8-178">Följande exempelkommandon visar hur du inaktiverar ett schema med Azure Service Management-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="887a8-178">The following sample commands show how to disable a schedule using the Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

<span data-ttu-id="887a8-179">Följande exempelkommandon visar hur du inaktiverar ett schema för en runbook med en Azure Resource Manager-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="887a8-179">The following sample commands show how to disable a schedule for a runbook using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a><span data-ttu-id="887a8-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="887a8-180">Next steps</span></span>
* <span data-ttu-id="887a8-181">Mer information om hur du arbetar med scheman finns [schema tillgångar i Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span><span class="sxs-lookup"><span data-stu-id="887a8-181">To learn more about working with schedules, see [Schedule Assets in Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span></span>
* <span data-ttu-id="887a8-182">Kom igång med runbooks i Azure Automation finns [starta en Runbook i Azure Automation](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="887a8-182">To get started with runbooks in Azure Automation, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md)</span></span> 

