---
title: aaaCreating eller importera en runbook i Azure Automation
description: "Den här artikeln beskriver hur toocreate en ny runbook i Azure Automation eller importera ett från en fil."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 24414362-b690-4474-8ca7-df18e30fc31d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte;bwren
ms.openlocfilehash: d45f44cf15fbcacdd0de2977668502c2e1671063
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Skapa eller importera en runbook i Azure Automation
Du kan lägga till en tooAzure runbook Automation genom att antingen [skapar en ny](#creating-a-new-runbook) eller genom att importera en befintlig runbook från en fil eller hello [Runbook-galleriet](automation-runbook-gallery.md). Den här artikeln innehåller information om hur du skapar och importerar runbooks från en fil.  Du kan hämta alla hello information om hur du använder community runbooks och moduler i [Azure Automation Runbook- och stänga](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Skapa en ny runbook
Du kan skapa en ny runbook i Azure Automation med någon av hello Azure portaler eller Windows PowerShell. När hello runbook har skapats kan du redigera den med hjälp av informationen i [Learning PowerShell-arbetsflöde](automation-powershell-workflow.md) och [grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md).

### <a name="toocreate-a-new-azure-automation-runbook-with-hello-azure-classic-portal"></a>toocreate en ny Azure Automation-runbook med hello Azure klassiska portal
Du kan endast arbeta med [PowerShell-arbetsflöde runbooks](automation-runbook-types.md#powershell-workflow-runbooks) i hello Azure-portalen.

1. I hello Azure klassiska portal, klickar på **ny**, **Apptjänster**, **Automation**, **Runbook**, **Snabbregistrering**.
2. Ange information om hello som krävs och klicka sedan på **skapa**. hello runbook-namn måste börja med en bokstav och kan innehålla bokstäver, siffror, understreck och bindestreck.
3. Om du vill tooedit hello runbook nu klickar **redigera Runbook**. Annars klickar du på **OK**.
4. Din nya runbook ska nu visas på hello **Runbooks** fliken.

### <a name="toocreate-a-new-azure-automation-runbook-with-hello-azure-portal"></a>toocreate en ny Azure Automation-runbook med hello Azure-portalen
1. Hello Azure-portalen, öppna ditt Automation-konto.
2. Hello Hub, Välj **Runbooks** tooopen hello lista över runbooks.
3. Klicka på hello **lägga till en runbook** knappen och sedan **skapa en ny runbook**.
4. Ange en **namn** hello runbook och välj dess [typen](automation-runbook-types.md). hello runbook-namn måste börja med en bokstav och kan innehålla bokstäver, siffror, understreck och bindestreck.
5. Klicka på **skapa** toocreate hello runbook och öppna hello-redigeraren.

### <a name="toocreate-a-new-azure-automation-runbook-with-windows-powershell"></a>toocreate en ny Azure Automation-runbook med Windows PowerShell
Du kan använda hello [ny AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) cmdlet toocreate en tom [PowerShell-arbetsflödesrunbook](automation-runbook-types.md#powershell-workflow-runbooks). Du kan antingen ange hello **namn** parametern toocreate en tom runbook att du kan redigera eller ange hello **sökväg** parametern tooimport en runbook-fil. Hej **typen** parametern bör också vara inkluderade toospecify hello fyra runbook typer.

hello följande exempel kommandon visar hur toocreate en ny tom runbook.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Importera en runbook från en fil till Azure Automation
Du kan skapa en ny runbook i Azure Automation genom att importera ett PowerShell-skript eller PowerShell-arbetsflöde (.ps1-tillägg) eller exporterade grafisk runbook (.graphrunbook).  Du måste ange hello [typ av runbook](automation-runbook-types.md) som skapas från hello import med hänsyn till kontot hello följande överväganden.

* En .graphrunbook-fil kan bara importeras till en ny [grafisk runbook](automation-runbook-types.md#graphical-runbooks), och grafiska runbook-flöden kan endast skapas från en .graphrunbook-fil.
* En .ps1-fil som innehåller ett PowerShell-arbetsflöde kan bara importeras till en [PowerShell-arbetsflödesrunbook](automation-runbook-types.md#powershell-workflow-runbooks).  Om hello-filen innehåller flera PowerShell-arbetsflöden, sedan misslyckas hello importen. Du måste spara varje arbetsflöde tooits egen fil och importera varje separat.
* En .ps1-fil som inte innehåller ett arbetsflöde kan importeras till antingen en [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) eller en [PowerShell-arbetsflödesrunbook](automation-runbook-types.md#powershell-workflow-runbooks).  Om den har importerats till en PowerShell-arbetsflödesrunbook, då blir konverterade tooa arbetsflödet och kommentarer ska inkluderas i hello runbook att ange hello ändringar som gjorts.

### <a name="tooimport-a-runbook-from-a-file-with-hello-azure-classic-portal"></a>tooimport en runbook från en fil med hello Azure klassiska portal
Du kan använda hello följa proceduren tooimport en skriptfil till Azure Automation.  Observera att du kan bara importera en .ps1-fil till en PowerShell-arbetsflödesrunbook med hjälp av den här portalen.  Du måste använda hello Azure-portalen för andra typer.

1. Välj i hello Azure-hanteringsportalen **Automation** och välj sedan ett Automation-konto.
2. Klicka på **Importera**.
3. Klicka på **Bläddra efter fil** och leta upp hello skriptet filen tooimport.
4. Om du vill tooedit hello runbook nu klickar **redigera Runbook**. Annars klickar du på OK.
5. hello nya runbook ska visas på hello **Runbooks** för hello Automation-konto.
6. Du måste [publicera hello runbook](#publishing-a-runbook) innan du kan köra den.

### <a name="tooimport-a-runbook-from-a-file-with-hello-azure-portal"></a>tooimport en runbook från en fil med hello Azure-portalen
Du kan använda hello följa proceduren tooimport en skriptfil till Azure Automation.  

> [!NOTE]
> Observera att du kan bara importera en .ps1-fil till en PowerShell-arbetsflödesrunbook med hello-portalen.
> 
> 

1. Hello Azure-portalen, öppna ditt Automation-konto.
2. Hello Hub, Välj **Runbooks** tooopen hello lista över runbooks.
3. Klicka på hello **lägga till en runbook** knappen och sedan **importera**.
4. Klicka på **Runbook-filen** tooselect hello filen tooimport
5. Om hello **namn** fältet har aktiverats och du har hello alternativet toochange den.  hello runbook-namn måste börja med en bokstav och kan innehålla bokstäver, siffror, understreck och bindestreck.
6. Hej [runbooktyp](automation-runbook-types.md) väljs automatiskt, men du kan ändra hello typ när du har tagit hello tillämpliga begränsningar i beräkningen. 
7. hello ny runbook visas i hello lista över runbooks för hello Automation-konto.
8. Du måste [publicera hello runbook](#publishing-a-runbook) innan du kan köra den.

> [!NOTE]
> När du importerar en grafisk runbook eller en grafisk PowerShell-arbetsflödesrunbook har du hello alternativet tooconvert toohello annan typ om du vill. Du kan inte konvertera tootextual.
> 
> 

### <a name="tooimport-a-runbook-from-a-script-file-with-windows-powershell"></a>tooimport en runbook från en skriptfil med Windows PowerShell
Du kan använda hello [importera AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) cmdlet tooimport en skriptfil som utkast PowerShell-arbetsflödesrunbook. Om det finns redan en hello runbook, hello import misslyckas om du inte använder hello *-Force* parameter. 

hello följande exempelkommandon visar hur tooimport ett skript-fil i en runbook.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Publicera en runbook
När du skapar eller importera en ny runbook, måste du publicera den innan du kan köra den.  Varje runbook i Automation har ett utkast och en publicerad version. Endast hello publicerade versionen är tillgänglig toobe kör och bara utkastet hello kan redigeras. hello publicerade versionen påverkas inte av några ändringar toohello utkastet. När hello utkastet ska göras tillgängligt publicerar du det som skriver över hello publicerade versionen med hello utkastet.

## <a name="toopublish-a-runbook-using-hello-azure-classic-portal"></a>toopublish en runbook med hello Azure klassiska portal
1. Öppna hello runbook i hello Azure klassiska portal.
2. Hello överkant hello-skärmen, klickar du på **författare**.
3. Hello ned hello-skärmen, klickar du på **publicera** och sedan **Ja** toohello verifieringsmeddelandet.

## <a name="toopublish-a-runbook-using-hello-azure-portal"></a>toopublish en runbook med hello Azure-portalen
1. Öppna hello runbook i hello Azure-portalen.
2. Klicka på hello **redigera** knappen.
3. Klicka på hello **publicera** knappen och sedan **Ja** toohello verifieringsmeddelandet.

## <a name="toopublish-a-runbook-using-windows-powershell"></a>toopublish en runbook med Windows PowerShell
Du kan använda hello [publicera AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) cmdlet toopublish en runbook med Windows PowerShell. hello följande exempel kommandon visar hur toopublish en exempel-runbook.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Nästa steg
* toolearn om hur du kan utnyttja hello Runbook och PowerShell-modulen galleriet finns [Azure Automation Runbook- och stänga](automation-runbook-gallery.md)
* toolearn mer information om hur du redigerar PowerShell och PowerShell-arbetsflöde runbooks med en textrepresentation editor finns [redigera textrepresentation runbooks i Azure Automation](automation-edit-textual-runbook.md)
* toolearn mer information om redigering av grafisk runbook finns [grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md)

