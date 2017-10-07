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
# <a name="scheduling-a-runbook-in-azure-automation"></a>Schemaläggning av en Runbook i Azure Automation
tooschedule en runbook i Azure Automation toostart vid en viss tidpunkt, du länka det tooone eller flera scheman. Ett schema kan vara konfigurerade tooeither köras en gång eller på ett reoccurring timvis eller daglig schema för runbooks i hello klassiska Azure-portalen och runbooks i hello Azure-portalen du kan också schemalägga dem för varje vecka, månad, specifika veckodagar hello eller dagar hello månaden, eller en viss dag i månaden hello.  En runbook kan vara länkad toomultiple scheman och ett schema kan ha flera runbooks länkade tooit.

## <a name="creating-a-schedule"></a>Skapa ett schema
Du kan skapa ett nytt schema för runbooks i hello Azure-portalen i hello klassiska portalen eller med Windows PowerShell. Du kan också ha hello möjlighet att skapa ett nytt schema när du länkar ett schema för tooa av runbook med hjälp av hello Azure klassiska eller Azure-portalen.

> [!NOTE]
> När du kopplar ett schema till en runbook Automation lagrar hello aktuella versioner av hello moduler i ditt konto och länkar dem toothat schema.  Det innebär att om du hade en modul med version 1.0 i ditt konto när du har skapat ett schema och sedan uppdaterar hello modulen tooversion 2.0 hello schema kommer att fortsätta toouse 1.0.  I ordning toouse hello uppdaterade Modulversion, måste du skapa ett nytt schema. 
> 
> 

### <a name="toocreate-a-new-schedule-in-hello-azure-classic-portal"></a>toocreate ett nytt schema i hello klassiska Azure-portalen
1. Markera Automation i hello klassiska Azure-portalen, och välj sedan hello namnet på ett automation-konto.
2. Välj hello **tillgångar** fliken.
3. Hello längst ned i hello-fönstret klickar du på **Lägg till inställning**.
4. Klicka på **Lägg till schema**.
5. Ange en **namn** och eventuellt en **beskrivning** för hello nya schedule.your schemat ska köras **en gång**, **timvis**, **Dagliga**, **veckovisa**, eller **månatliga**.
6. Ange en **starttid** och andra alternativ beroende på hello typ av schema som du har valt.

### <a name="toocreate-a-new-schedule-in-hello-azure-portal"></a>toocreate ett nytt schema i hello Azure-portalen
1. I hello Azure-portalen från ditt automation-konto klickar du på hello **tillgångar** panelen tooopen hello **tillgångar** bladet.
2. Klicka på hello **scheman** panelen tooopen hello **scheman** bladet.
3. Klicka på **lägga till ett schema** hello överst i hello-bladet.
4. På hello **nytt schema** bladet typ a **namn** och eventuellt en **beskrivning** för hello nytt schema.
5. Välj om hello schemat ska köras en gång, eller på ett reoccurring schema genom att välja **när** eller **återkommande**.  Om du väljer **när** ange en **starttid** och klicka sedan på **skapa**.  Om du väljer **återkommande**, ange en **starttid** och hello frekvens för hur ofta du vill hello runbook toorepeat - av **timme**, **dag**, **vecka**, eller av **månad**.  Om du väljer **vecka** eller **månad** hello nedrullningsbara listan hello **upprepning alternativet** visas i bladet hello och vid val av hello **upprepning alternativet** bladet visas och du kan välja hello veckodag om du har valt **vecka**.  Om du har valt **månad**, du kan välja efter **veckodagar** eller särskilda dagar i månaden hello på hello kalender och slutligen vill du toorun på hello sista dagen i månaden hello eller inte och klicka sedan på **OK** .   

### <a name="toocreate-a-new-schedule-with-windows-powershell"></a>toocreate ett nytt schema med Windows PowerShell
Du kan använda hello [ny AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet toocreate ett nytt schema i Azure Automation för klassiska runbooks eller [ny AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet för runbooks i hello Azure portalen. Du måste ange hello starttiden för hello schema och hello frekvens som ska köras.

hello följande exempelkommandon visar hur toocreate ett nytt schema som körs varje dag klockan 3:30 startar på 20 januari 2015 med Azure Service Management-cmdlet.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

hello följande exempel kommandon visar hur toocreate ett schema för hello 15 och 30: e i månaden med en Azure Resource Manager-cmdlet.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"


## <a name="linking-a-schedule-tooa-runbook"></a>Länka ett schema tooa runbook
En runbook kan vara länkad toomultiple scheman och ett schema kan ha flera runbooks länkade tooit. Om en runbook har parametrar, kan du ange värden för dessa. Du måste ange värden för alla obligatoriska parametrar och kan ange värden för valfria parametrar.  Dessa värden kommer att användas varje gång hello runbook har startats med det här schemat.  Du kan koppla hello samma runbook tooanother schema och ange olika parametervärden.

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-classic-portal"></a>toolink en schema tooa runbook med hello klassiska Azure-portalen
1. Välj i hello klassiska Azure-portalen, **Automation** och klicka sedan på hello namnet på ett automation-konto.
2. Välj hello **Runbooks** fliken.
3. Klicka på hello namnet på hello runbook tooschedule.
4. Klicka på hello **schema** fliken.
5. Om hello runbook inte är för närvarande länkade tooa schema, så får du hello alternativet för**länka tooa nytt schema** eller **länka tooan befintliga schema**.  Om hello runbook är för närvarande länkade tooa schema, klickar du på **länk** på hello längst ned på hello fönstret tooaccess dessa alternativ.
6. Om hello runbook har parametrar uppmanas för deras värden.  

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-portal"></a>toolink en schema tooa runbook med hello Azure-portalen
1. I hello Azure-portalen från ditt automation-konto klickar du på hello **Runbooks** panelen tooopen hello **Runbooks** bladet.
2. Klicka på hello namnet på hello runbook tooschedule.
3. Om hello runbook inte är för närvarande länkade tooa schema, kommer du att angivna hello alternativet toocreate ett nytt schema eller länka tooan befintliga schema.  
4. Om hello runbook har parametrar, kan du välja alternativet hello **ändra körningsinställningar (standard: Azure)** och hello **parametrar** bladet visas där du kan ange hello information i enlighet med detta.  

### <a name="toolink-a-schedule-tooa-runbook-with-windows-powershell"></a>toolink ett schema tooa runbook med Windows PowerShell
Du kan använda hello [registrera AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) toolink runbook ett schema tooa klassiska eller [registrera AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet för runbooks i hello Azure-portalen.  Du kan ange värden för hello runbook-parametrar med hello parametrar parameter. Se [starta en Runbook i Azure Automation](automation-starting-a-runbook.md) mer information om specificering av parametervärden.

hello följande exempel kommandon visar hur toolink ett schema med en Azure Service Management-cmdlet med parametrar.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

hello följande exempel kommandon visar hur toolink en schema tooa runbook med en Azure Resource Manager-cmdlet med parametrar.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a>Inaktivera ett schema
När du inaktiverar ett schema körs inte längre alla runbooks som är länkade tooit på schemat. Du kan manuellt inaktivera ett schema eller ange en förfallotid för scheman med en frekvens när de skapas. När hello giltighetstid har uppnåtts, ska hello schemat inaktiveras.

### <a name="toodisable-a-schedule-from-hello-azure-classic-portal"></a>toodisable ett schema från hello klassiska Azure-portalen
Du kan inaktivera ett schema i hello klassiska Azure-portalen från hello Schemadetaljer sida för hello schema.

1. Välj Automation i hello klassiska Azure-portalen, och klicka sedan på hello namnet på ett automation-konto.
2. Fliken hello tillgångar.
3. Klicka på ett schema tooopen hello namn dess detaljsida.
4. Ändra **aktiverad** för**nr**.

### <a name="toodisable-a-schedule-from-hello-azure-portal"></a>toodisable ett schema från hello Azure-portalen
1. I hello Azure-portalen från ditt automation-konto klickar du på hello **tillgångar** panelen tooopen hello **tillgångar** bladet.
2. Klicka på hello **scheman** panelen tooopen hello **scheman** bladet.
3. Klicka på ett schema tooopen hello informationsbladet hello namn.
4. Ändra **aktiverad** för**nr**.

### <a name="toodisable-a-schedule-with-windows-powershell"></a>toodisable ett schema med Windows PowerShell
Du kan använda hello [Set AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet toochange hello egenskaper för ett befintligt schema för en klassiska runbook eller [Set AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet för runbooks i hello Azure portalen. toodisable hello schema, ange **FALSKT** för hello **IsEnabled** parameter.

hello följande exempelkommandon visar hur toodisable ett schema med hello Azure Service Management-cmdlet.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

hello följande exempel kommandon visar hur toodisable ett schema för en runbook med en Azure Resource Manager-cmdlet.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a>Nästa steg
* toolearn mer information om hur du arbetar med scheman, se [schema tillgångar i Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)
* tooget igång med runbooks i Azure Automation finns [starta en Runbook i Azure Automation](automation-starting-a-runbook.md) 

