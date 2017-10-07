---
title: aaaEditing textrepresentation runbooks i Azure Automation
description: "Den här artikeln innehåller olika procedurer för att arbeta med PowerShell och PowerShell-arbetsflöde runbooks i Azure Automation med hello textrepresentation redigeraren."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: 6f5b48fb-6f30-4e99-9e14-9061b5554b08
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 3fd87d457838f300ca6c94bc345e82c679a0e011
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Redigera textrepresentation runbooks i Azure Automation
hello textrepresentation redigeraren i Azure Automation kan vara används tooedit [PowerShell runbooks](automation-runbook-types.md#powershell-runbooks) och [PowerShell-arbetsflöde runbooks](automation-runbook-types.md#powershell-workflow-runbooks). Detta har hello vanliga funktioner i andra koden redigerare, till exempel intellisense och skriva kod med ytterligare särskilda funktioner tooassist du åtkomst till resurser vanliga toorunbooks färg.  Den här artikeln innehåller detaljerade anvisningar för att utföra olika funktioner med den här redigeraren.

textrepresentation hello-redigeraren innehåller en funktion tooinsert kod för aktiviteter, resurser och underordnade runbooks i en runbook. I stället för att skriva i hello koden själv, kan du välja från en lista över tillgängliga resurser och har hello infoga lämplig kod i hello runbook.

Varje runbook i Azure Automation har två versioner: utkast och publicerad. Du redigerar hello utkastversionen för hello runbook och publicerar det så att den kan köras. hello publicerade versionen kan inte redigeras. Se [publicera en runbook](automation-creating-importing-runbook.md#publishing-a-runbook) för mer information.

toowork med [grafiska runbook-flöden](automation-runbook-types.md#graphical-runbooks), se [grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md).

## <a name="tooedit-a-runbook-with-hello-azure-portal"></a>tooedit en runbook med hello Azure-portalen
Använd hello följa proceduren tooopen en runbook för redigering i textrepresentation hello-redigeraren.

1. Välj ditt automation-konto i hello Azure-portalen.
2. Klicka på hello **Runbooks** panelen tooopen hello lista över runbooks.
3. Klicka på hello namn hello runbook du vill tooedit och klickar sedan på hello **redigera** knappen.
4. Utföra hello som krävs för redigering.
5. Klicka på **spara** när ändringarna har slutförts.
6. Klicka på **publicera** om du vill att hello senaste utkastversionen för hello runbook toobe publiceras.

### <a name="tooinsert-a-cmdlet-into-a-runbook"></a>tooinsert en cmdlet i en runbook
1. Placera hello markören där du vill att tooplace hello cmdlet i hello arbetsytan hello textrepresentation Editor.
2. Expandera hello **Cmdlets** nod i hello biblioteket kontroll.
3. Expandera hello-modul som innehåller hello-cmdlet som du vill ha toouse.
4. Högerklicka på hello cmdlet tooinsert och välj **lägga till toocanvas**.  Om hello-cmdlet har mer än en parameter har angetts, läggs hello standarduppsättning.  Du kan också expandera hello cmdlet tooselect en annan parameter angetts.
5. hello-koden för hello cmdlet infogas med hela listan över parametrar.
6. Ange ett lämpligt värde i stället hello-datatypen som omges av klammerparenteser <> för alla obligatoriska parametrar.  Ta bort alla parametrar som du behöver.

### <a name="tooinsert-code-for-a-child-runbook-into-a-runbook"></a>tooinsert koden för en underordnad runbook i en runbook
1. I hello arbetsytan hello textrepresentation Editor, placera hello markören där du vill tooplace hello koden för hello [underordnad runbook](automation-child-runbooks.md).
2. Expandera hello **Runbooks** nod i hello biblioteket kontroll.
3. Högerklicka på hello runbook tooinsert och välj **lägga till toocanvas**.
4. hello-koden för hello underordnad runbook infogas med några platshållare för alla runbookparametrar.
5. Ersätt hello-platshållare med lämpliga värden för varje parameter.

### <a name="tooinsert-an-asset-into-a-runbook"></a>tooinsert en tillgång till en runbook
1. Placera hello markören där du vill tooplace hello koden för hello underordnad runbook i hello arbetsytan hello textrepresentation Editor.
2. Expandera hello **tillgångar** nod i hello biblioteket kontroll.
3. Expandera hello noden för hello typ av tillgång som du vill använda.
4. Högerklicka på hello tillgången tooinsert och välj **lägga till toocanvas**.  För [variabeln tillgångar](automation-variables.md), väljer du antingen **Lägg till ”hämta variabel” toocanvas** eller **Lägg till ”ange variabel” toocanvas** beroende på om du vill tooget eller ange hello variabel.
5. hello-kod för hello tillgång infogas hello runbook.

## <a name="tooedit-a-runbook-with-hello-azure-portal"></a>tooedit en runbook med hello Azure-portalen
Använd hello följa proceduren tooopen en runbook för redigering i textrepresentation hello-redigeraren.

1. Välj i hello Azure-portalen, **Automation** och klicka sedan på hello namnet på ett automation-konto.
2. Välj hello **Runbooks** fliken.
3. Klicka hello namnet i hello runbook du vill tooedit och välj sedan hello **författare** fliken.
4. Klicka på hello **redigera** knappen längst ned hello hello-skärmen.
5. Utföra hello som krävs för redigering.
6. Klicka på **spara** när ändringarna har slutförts.
7. Klicka på **publicera** om du vill att hello senaste utkastversionen för hello runbook toobe publiceras.

### <a name="tooinsert-an-activity-into-a-runbook"></a>tooinsert en aktivitet i en Runbook
1. Placera hello markören där du vill tooplace hello aktiviteter i hello arbetsytan hello textrepresentation Editor.
2. Hello ned hello-skärmen, klickar du på **infoga** och sedan **aktiviteten**.
3. I hello **Integrationsmodul** kolumn, Välj hello-modul som innehåller hello-aktivitet.
4. I hello **aktiviteten** väljer du en aktivitet.
5. I hello **beskrivning** kolumnen Obs hello beskrivning av hello-aktivitet. Alternativt kan du välja att visa detaljerad hjälp toolaunch hello aktivitet i hello webbläsare.
6. Klicka på högerpilen för hello.  Om hello aktiviteten har parametrar visas de för kännedom.
7. Klicka på knappen för hello.  Koden toorun hello aktiviteten kommer att infogas i hello runbook.
8. Om hello aktiviteten kräver parametrar anger du ett lämpligt värde i stället för hello-datatypen som omges av klammerparenteser <>.

### <a name="tooinsert-code-for-a-child-runbook-into-a-runbook"></a>tooinsert koden för en underordnad runbook i en runbook
1. I hello arbetsytan hello textrepresentation Editor, placera hello markören där du vill tooplace hello [underordnad runbook](automation-child-runbooks.md).
2. Hello ned hello-skärmen, klickar du på **infoga** och sedan **Runbook**.
3. Välj hello runbook tooinsert hello mellersta kolumnen och klicka på högerpilen för hello.
4. Om hello runbook har parametrar visas de för kännedom.
5. Klicka på knappen för hello.  Koden toorun hello markerad runbook kommer att infogas i hello aktuell runbook.
6. Om hello runbook kräver parametrar anger du ett lämpligt värde i stället för hello-datatypen som omges av klammerparenteser <>.

### <a name="tooinsert-an-asset-into-a-runbook"></a>tooinsert en tillgång till en runbook
1. Placera i hello arbetsytan hello textrepresentation Editor hello markören där du vill att tooplace hello aktiviteten tooretrieve hello tillgången.
2. Hello ned hello-skärmen, klickar du på **infoga** och sedan **inställningen**.
3. I hello **Inställningsåtgärd** kolumn, Välj hello-åtgärd som du vill.
4. Välj hello tillgängliga resurserna i hello mellersta kolumnen.
5. Klicka på knappen för hello.  Code tooget eller ange hello tillgången kommer att infogas i hello runbook.

## <a name="tooedit-an-azure-automation-runbook-using-windows-powershell"></a>tooedit en Azure Automation-runbook med Windows PowerShell
tooedit en runbook med Windows PowerShell kan du använda hello redigeringsprogram och spara den tooa .ps1-fil. Du kan använda hello [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) cmdlet tooretrieve hello innehållet i hello runbook och sedan [Set AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) cmdlet tooreplace hello befintliga utkast-runbooken med hello ändra något.

### <a name="tooretrieve-hello-contents-of-a-runbook-using-windows-powershell"></a>tooRetrieve hello innehållet i en Runbook med Windows PowerShell
hello följande exempelkommandon visar hur tooretrieve hello skriptet för en runbook och spara den tooa skriptfilen. I det här exemplet hämtas utkastversionen hello. Det är också möjligt tooretrieve hello publicerade versionen av hello runbook även om den här versionen inte kan ändras.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="toochange-hello-contents-of-a-runbook-using-windows-powershell"></a>tooChange hello innehållet i en Runbook med Windows PowerShell
hello följande exempelkommandon visar hur tooreplace hello befintliga innehållet i en runbook med hello innehållet i en skriptfil. Observera att detta är hello samma procedur som i exempel [tooimport en runbook från en skriptfil med Windows PowerShell](automation-creating-importing-runbook.md).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Relaterade artiklar
* [Skapa eller importera en runbook i Azure Automation](automation-creating-importing-runbook.md)
* [Learning PowerShell-arbetsflöde](automation-powershell-workflow.md)
* [Grafisk redigering i Azure Automation](automation-graphical-authoring-intro.md)
* [Certifikat](automation-certificates.md)
* [Anslutningar](automation-connections.md)
* [Autentiseringsuppgifter](automation-credentials.md)
* [Scheman](automation-schedules.md)
* [Variabler](automation-variables.md)
