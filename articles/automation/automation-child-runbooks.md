---
title: aaaChild runbooks i Azure Automation | Microsoft Docs
description: "Beskriver hello olika metoder för att starta en runbook i Azure Automation från en annan runbook och dela information med varandra."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 919887b9-43e2-4c16-883c-f81807fe37db
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2017
ms.author: magoedte;bwren
ms.openlocfilehash: d3d06818d344b565d53cc4f4705b41dcfcf9a376
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="child-runbooks-in-azure-automation"></a>Underordnade runbooks i Azure Automation
Det är ett metodtips i Azure Automation toowrite återanvändningsbara, modulbaserade runbooks med en diskret funktion som kan användas av andra runbooks. En överordnad runbook anropar ofta en eller flera underordnade runbooks tooperform som krävs för funktioner. Det finns två sätt toocall en underordnad runbook och var och en har tydliga skillnader som du bör känna till så att du kan bestämma vilket som är bäst för dina olika scenarier.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Anropa en underordnad runbook med infogad körning
tooinvoke en infogad runbook från en annan runbook du använder hello namnet på hello runbook och ange värden för parametrarna exakt samma sätt som du använder en aktivitet eller cmdlet.  Alla runbooks i hello samma Automation-konto är tillgängliga tooall andra toobe används i det här sättet. hello överordnad runbook väntar hello underordnade runbook toocomplete innan du flyttar toohello nästa rad och eventuella utdata returneras direkt toohello överordnade.

När du aktiverar en infogad runbook körs den i hello samma jobb som hello överordnad runbook. Det kommer inte att visa hello jobbhistorik för hello underordnad runbook som kördes. Eventuella undantag och strömmad utdata från hello underordnade runbooken kommer att associeras med hello överordnade. Detta innebär färre jobb och gör dem lättare tootrack och tootroubleshoot eftersom alla undantag från hello underordnad runbook och alla strömmad utdata är associerade med hello överordnade jobb.

När en runbook publiceras måste alla underordnade runbooks som anropas redan publiceras. Det beror på att Azure Automation bygger en association med någon av underordnade runbooks när en runbook kompileras. Om de inte är hello överordnad runbook visas toopublish korrekt, men genererar ett undantag när den startas. Om det händer kan publicera du hello överordnad runbook i ordning tooproperly referens hello underordnade runbooks. Du behöver inte toorepublish hello överordnade runbooken om någon av hello underordnade runbooks ändras eftersom hello association kommer redan har skapats.

hello parametrar för en underordnad runbook som anropas internt kan vara en datatyp som inkluderar komplexa objekt och det finns inga [JSON-serialisering](automation-starting-a-runbook.md#runbook-parameters) eftersom det inte finns när du startar hello runbook med hello Azure-hanteringsportalen eller med hello Start-AzureRmAutomationRunbook cmdlet.

### <a name="runbook-types"></a>Runbook-typer
Vilka typer kan anropa varandra:

* En [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) och [grafiska runbook-flöden](automation-runbook-types.md#graphical-runbooks) kan anropa varje infogad som (båda är PowerShell-baserat).
* En [PowerShell-arbetsflödesrunbook](automation-runbook-types.md#powershell-workflow-runbooks) och grafisk PowerShell-arbetsflöde runbooks kan anropa varje infogad som (båda är PowerShell-arbetsflöde baserat)
* Hej PowerShell typer och hello PowerShell-arbetsflöde typer kan inte anropa varandra infogade och måste använda Start AzureRmAutomationRunbook.

När du publicera ordning frågan:

* hello publicera ordningen för runbooks som endast är viktigt för PowerShell-arbetsflödet och grafisk PowerShell-arbetsflöde runbooks.

När du anropar en grafisk eller PowerShell-arbetsflöde underordnad runbook med infogad körning, använder du bara hello namnet på hello runbook.  När du anropar en underordnad runbook som PowerShell, du måste föregås namnet med *.\\*  toospecify som hello skript finns i hello lokala katalog. 

### <a name="example"></a>Exempel
hello följande exempel anropar ett test en underordnad runbook som accepterar tre parametrar, ett komplext objekt, ett heltal och ett booleskt värde. hello utdata från hello underordnade runbooken tilldelas tooa variabeln.  I det här fallet är hello underordnad runbook ett PowerShell-arbetsflödesrunbook

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Följande är hello samma exempel med hjälp av en PowerShell-runbook som hello underordnad.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true


## <a name="starting-a-child-runbook-using-cmdlet"></a>Starta en underordnad runbook med hjälp av cmdlet
Du kan använda hello [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) cmdlet toostart en runbook enligt beskrivningen i [toostart en runbook med Windows PowerShell](automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Det finns två lägen för användning av denna cmdlet.  I ett läge returnerar hello cmdlet hello jobb-id som hello underordnade jobb skapas för hello underordnad runbook.  Hej i andra läge, som du aktiverar genom att ange hello **-vänta** parametern hello cmdlet ska vänta tills hello underordnade jobbet har slutförts och returnerar hello utdata från hello underordnad runbook.

hello jobb från en underordnad runbook som startas med en cmdlet kommer att köras i ett separat jobb från hello överordnad runbook. Detta innebär fler jobb än anropas infogad för hello runbook och gör dem svårare tootrack. hello överordnade kan starta flera underordnade runbooks asynkront utan att vänta på varje toocomplete. För att få samma typ av parallell körning vid anrop av infogade för hello underordnade runbooks, hello överordnad runbook måste toouse hello [parallellt nyckelord](automation-powershell-workflow.md#parallel-processing).

Parametrar för en underordnad runbook som startas med en cmdlet tillhandahålls som en hash-tabell enligt beskrivningen i [Runbookparametrar](automation-starting-a-runbook.md#runbook-parameters). Endast enkla datatyper kan användas. Om hello runbook har en parameter med en komplex datatyp, det måste den anropas infogad.

### <a name="example"></a>Exempel
hello följande exempel startas en underordnad runbook med parametrar och sedan väntar toocomplete med hello Start AzureRmAutomationRunbook-vänta parametern. När slutförd samlas utdata från hello underordnad runbook.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResourceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Jämförelse av metoder för att anropa en underordnad runbook
hello sammanfattas följande tabell hello skillnaderna mellan hello två metoder för att anropa en runbook från en annan runbook.

|  | Infogade | Cmdlet |
|:--- |:--- |:--- |
| Jobb |Underordnade runbooks körs i samma jobb som överordnade hello hello. |Ett separat jobb skapas för hello underordnad runbook. |
| Körning |Överordnad runbook väntar hello underordnade runbook toocomplete innan du fortsätter. |Överordnad runbook fortsätter omedelbart efter att underordnad runbook startas *eller* överordnad runbook väntar hello underordnade jobbet toofinish. |
| Resultat |Överordnad runbook kan hämta utdata direkt från underordnad runbook. |Överordnad runbook måste hämta utdata från underordnat runbook-jobb *eller* överordnad runbook kan hämta utdata direkt från underordnad runbook. |
| Parametrar |Värden för parametrar som hello underordnad runbook anges separat och kan använda alla datatyper. |Värdena för hello underordnad runbook parametrar måste kombineras i en enda hash-tabell och får bara innehålla enkla, matris och objekt-datatyper som använder JSON-serialisering. |
| Automation-konto |Överordnad runbook kan bara använda underordnad runbook i hello samma automation-konto. |Överordnad runbook kan använda underordnad runbook från en automation-konto från hello samma Azure-prenumeration och en annan prenumeration även om du har en anslutning tooit. |
| Publicering |Underordnad runbook måste publiceras innan överordnad runbook publiceras. |Underordnad runbook måste publiceras innan överordnad runbook startas. |

## <a name="next-steps"></a>Nästa steg
* [Starta en runbook i Azure Automation](automation-starting-a-runbook.md)
* [Runbook-utdata och meddelanden i Azure Automation](automation-runbook-output-and-messages.md)

