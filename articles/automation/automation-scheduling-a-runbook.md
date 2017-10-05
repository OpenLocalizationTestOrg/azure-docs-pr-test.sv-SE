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
# <a name="scheduling-a-runbook-in-azure-automation"></a>Schemaläggning av en Runbook i Azure Automation
Om du vill schemalägga en runbook i Azure Automation för att starta vid en viss tidpunkt länka du det till ett eller flera scheman. Ett schema kan konfigureras för att köras en gång eller på en igen timvis eller daglig schema för runbooks i den klassiska Azure-portalen och runbooks i Azure-portalen, kan du dessutom schemalägga dem för varje vecka, månad, särskilda dagar i veckan eller dagar efter m h eller en viss dag i månaden.  En runbook kan länkas till flera scheman och ett schema kan ha flera runbooks som är länkade till den.

## <a name="creating-a-schedule"></a>Skapa ett schema
Du kan skapa ett nytt schema för runbooks i Azure-portalen i den klassiska portalen eller med Windows PowerShell. Du har också möjlighet att skapa ett nytt schema när du länkar en runbook till ett schema med Azure klassiska eller Azure-portalen.

> [!NOTE]
> När du kopplar ett schema till en runbook Automation lagrar aktuella versioner av moduler i ditt konto och kopplar dem till schemat.  Det innebär att om du har en modul med version 1.0 i ditt konto när du har skapat ett schema och uppdatera modulen till version 2.0, schemat kommer att fortsätta att använda 1.0.  För att kunna använda den uppdaterade modulversionen, måste du skapa ett nytt schema. 
> 
> 

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Skapa ett nytt schema i den klassiska Azure-portalen
1. Markera Automation och välj sedan namnet på ett automation-konto i den klassiska Azure-portalen.
2. Välj den **tillgångar** fliken.
3. Längst ned i fönstret klickar du på **Lägg till inställning**.
4. Klicka på **Lägg till schema**.
5. Ange en **namn** och eventuellt en **beskrivning** för nya schedule.your schemat ska köras **en gång**, **timvis**, **dagliga**, **veckovisa**, eller **månatliga**.
6. Ange en **starttid** och andra alternativ beroende på vilken typ av schema som du har valt.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Skapa ett nytt schema i Azure-portalen
1. I Azure-portalen från ditt automation-konto klickar du på den **tillgångar** öppna den **tillgångar** bladet.
2. Klicka på den **scheman** öppna den **scheman** bladet.
3. Klicka på **lägga till ett schema** längst upp på bladet.
4. På den **nytt schema** bladet typ a **namn** och eventuellt en **beskrivning** för det nya schemat.
5. Välj om schemat ska köras en gång, eller på ett reoccurring schema genom att välja **när** eller **återkommande**.  Om du väljer **när** ange en **starttid** och klicka sedan på **skapa**.  Om du väljer **återkommande**, ange en **starttid** och frekvens för hur ofta du vill att runbook ska upprepas - av **timme**, **dag**, **vecka**, eller av **månad**.  Om du väljer **vecka** eller **månad** från den nedrullningsbara listan den **upprepning alternativet** visas i bladet och vid val av den **upprepning alternativet** bladet visas och du kan välja dag i veckan om du har valt **vecka**.  Om du har valt **månad**, du kan välja efter **veckodagar** eller särskilda dagar i månaden i kalendern och slutligen vill du köra den på den sista dagen i månaden eller inte och klicka sedan på **OK**.   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Skapa ett nytt schema med Windows PowerShell
Du kan använda den [ny AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) för att skapa ett nytt schema i Azure Automation för klassiska runbooks eller [ny AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet för runbooks i Azure-portalen. Du måste ange starttiden för schemat och frekvensen ska köras.

Följande exempelkommandon visar hur du skapar ett nytt schema som körs varje dag klockan 3:30 startar på 20 januari 2015 med Azure Service Management-cmdlet.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

Följande exempelkommandon visar hur du skapar ett schema för den 15: e och 30: e i månaden med en Azure Resource Manager-cmdlet.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"


## <a name="linking-a-schedule-to-a-runbook"></a>Länka ett schema till en runbook
En runbook kan länkas till flera scheman och ett schema kan ha flera runbooks som är länkade till den. Om en runbook har parametrar, kan du ange värden för dessa. Du måste ange värden för alla obligatoriska parametrar och kan ange värden för valfria parametrar.  Dessa värden används varje gång runbook startas med det här schemat.  Du kan koppla samma runbook till ett annat schema och ange olika parametervärden.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Länka ett schema till en runbook med den klassiska Azure-portalen
1. I den klassiska Azure-portalen väljer **Automation** och klicka sedan på namnet på ett automation-konto.
2. Välj den **Runbooks** fliken.
3. Klicka på namnet på runbooken som ska schemaläggas.
4. Klicka på den **schema** fliken.
5. Om runbook inte är för närvarande kopplad till ett schema, så får du alternativet att **länk till ett nytt schema** eller **länk till ett befintligt schema**.  Om runbooken för närvarande är länkad till ett schema, klickar du på **länk** längst ned i fönstret för att komma åt dessa alternativ.
6. Om runbooken har parametrar, att du uppmanas deras värden.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Länka ett schema till en runbook med Azure-portalen
1. I Azure-portalen från ditt automation-konto klickar du på den **Runbooks** öppna den **Runbooks** bladet.
2. Klicka på namnet på runbooken som ska schemaläggas.
3. Om runbook inte är för närvarande kopplad till ett schema, sedan ges du möjlighet att skapa ett nytt schema eller länka till ett befintligt schema.  
4. Om runbooken har parametrar, kan du välja alternativet **ändra körningsinställningar (standard: Azure)** och **parametrar** bladet visas där du kan ange information i enlighet med detta.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Länka ett schema till en runbook med Windows PowerShell
Du kan använda den [registrera AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) länka ett schema till en klassisk runbook eller [registrera AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet för runbooks i Azure-portalen.  Du kan ange värden för runbookens parametrar med parametern parametrar. Se [starta en Runbook i Azure Automation](automation-starting-a-runbook.md) mer information om specificering av parametervärden.

Följande exempelkommandon visar hur du länkar ett schema med en Azure Service Management-cmdlet med parametrar.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

Följande exempelkommandon visar hur du länkar ett schema till en runbook med en Azure Resource Manager-cmdlet med parametrar.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a>Inaktivera ett schema
När du inaktiverar ett schema körs inte längre alla runbooks som är kopplade till schemat. Du kan manuellt inaktivera ett schema eller ange en förfallotid för scheman med en frekvens när de skapas. Schemat kommer att inaktiveras när förfallotid har uppnåtts.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Inaktivera ett schema från den klassiska Azure-portalen
Du kan inaktivera ett schema i den klassiska Azure-portalen från sidan Schemadetaljer för schemat.

1. Välj Automation och klicka sedan på namnet på ett automation-konto i den klassiska Azure-portalen.
2. Välj fliken tillgångar.
3. Klicka på namnet på ett schema för att öppna dess detaljsida.
4. Ändra **aktiverat** till **nr**.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Inaktivera ett schema från Azure-portalen
1. I Azure-portalen från ditt automation-konto klickar du på den **tillgångar** öppna den **tillgångar** bladet.
2. Klicka på den **scheman** öppna den **scheman** bladet.
3. Klicka på namnet på ett schema för att öppna informationsbladet.
4. Ändra **aktiverat** till **nr**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Inaktivera ett schema med Windows PowerShell
Du kan använda den [Set AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) för att ändra egenskaperna för ett befintligt schema för en klassiska runbook eller [Set AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet för runbooks i Azure-portalen. Om du vill inaktivera schemat, ange **FALSKT** för den **IsEnabled** parameter.

Följande exempelkommandon visar hur du inaktiverar ett schema med Azure Service Management-cmdlet.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

Följande exempelkommandon visar hur du inaktiverar ett schema för en runbook med en Azure Resource Manager-cmdlet.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a>Nästa steg
* Mer information om hur du arbetar med scheman finns [schema tillgångar i Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)
* Kom igång med runbooks i Azure Automation finns [starta en Runbook i Azure Automation](automation-starting-a-runbook.md) 

