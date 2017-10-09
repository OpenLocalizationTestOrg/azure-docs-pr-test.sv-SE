---
title: "aaaRunbook körning i Azure Automation | Microsoft Docs"
description: Beskriver hello information om hur en runbook i Azure Automation bearbetas.
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d10c8ce2-2c0b-4ea7-ba3c-d20e09b2c9ca
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/17/2017
ms.author: bwren
ms.openlocfilehash: bdb535675443353d44640bc7773de3f9dac5e42c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-execution-in-azure-automation"></a>Runbook-körningen i Azure Automation
När du startar en runbook i Azure Automation skapas ett jobb. Ett jobb är en enskild körningsinstans av en runbook. En Azure Automation arbetare är tilldelad toorun varje jobb. När anställda delas av flera Azure-konton, är jobb från olika Automation-konton isolerade från varandra. Du har inte kontroll över vilka worker services hello-begäran för jobbet.  En enstaka runbook kan ha flera jobb körs samtidigt. När du visar hello lista med runbooks i hello Azure-portalen visar hello status för alla jobb som har startats för varje runbook. Du kan visa hello lista över jobb för varje runbook i ordning tootrack hello status för alla. En beskrivning av olika jobbstatus för hello finns [jobbstatus](#job-statuses).

hello följande diagram visar hello livscykeln för ett runbook-jobb för [grafiska runbook-flöden](automation-runbook-types.md#graphical-runbooks) och [PowerShell-arbetsflöde runbooks](automation-runbook-types.md#powershell-workflow-runbooks).

![Jobbstatus - PowerShell-arbetsflöde](./media/automation-runbook-execution/job-statuses.png)

hello följande diagram visar hello livscykeln för ett runbook-jobb för [PowerShell runbooks](automation-runbook-types.md#powershell-runbooks).

![Jobbstatus - PowerShell-skript](./media/automation-runbook-execution/job-statuses-script.png)

Jobb har åtkomst tooyour Azure resurser genom att göra en anslutning tooyour Azure-prenumeration. De har bara åtkomst tooresources i ditt datacenter om resurserna är tillgängliga från hello offentligt moln.

## <a name="job-statuses"></a>Jobbstatus
hello beskrivs följande tabell hello olika statuslägen som är möjliga för ett jobb.

| Status | Beskrivning |
|:--- |:--- |
| Slutfört |hello-jobbet har slutförts. |
| Det gick inte |För [grafisk och PowerShell-arbetsflöde runbooks](automation-runbook-types.md), hello runbook misslyckades toocompile.  För [PowerShell-skript runbooks](automation-runbook-types.md), hello runbook misslyckades toostart eller hello jobbet påträffade ett undantag. |
| Misslyckades, väntar på resurser |hello-jobbet misslyckades eftersom den nått hello [fördelning](#fairshare) begränsa tre gånger och startats från hello samma kontrollpunkten eller från hello av hello runbook startas varje gång. |
| I kö |hello jobbet väntar på resurser på en Automation arbetare toocome tillgängliga så att den kan startas. |
| Startar |hello-jobbet har tilldelats tooa arbetare och hello systemet är i hello processen för att starta den. |
| Återuppta |hello systemet är i hello processen för att återuppta hello jobbet när det pausades. |
| Körs |hello jobbet körs. |
| Kör, väntar på resurser |hello jobbet har tagits bort eftersom den nått hello [fördelning](#fairshare) gränsen. Återupptas från den senaste kontrollpunkten inom kort. |
| Stoppad |hello jobbet stoppades av användaren hello innan den slutfördes. |
| Stoppas |hello systemet är i hello processen att stoppa hello jobb. |
| avbruten |hello jobbet pausades av hello användare, hello system eller ett kommando i hello runbook. Ett jobb som pausats kan startas igen och återuppta från den senaste kontrollpunkten eller från hello i början av hello runbook om den inte har kontrollpunkter. Hej runbook pausas bara av hello systemet när ett undantag inträffar. ErrorActionPreference är som standard för**Fortsätt**, vilket innebär att hello jobbet körs på ett fel. Om inställningsvariabeln är inställd för**stoppa**, och sedan hello jobbet pausar på ett fel.  Gäller för[grafisk och PowerShell-arbetsflöde runbooks](automation-runbook-types.md) endast. |
| Pausa |hello systemet försöker toosuspend hello jobb på begäran, hello hello användare. Hej runbooken måste nå nästa kontrollpunkt innan den kan pausas. Om den redan passerat den sista kontrollpunkten sedan är klar innan den kan pausas.  Gäller för[grafisk och PowerShell-arbetsflöde runbooks](automation-runbook-types.md) endast. |

## <a name="viewing-job-status-from-hello-azure-portal"></a>Visa jobbstatus från hello Azure-portalen
Du kan visa en sammanfattande status för alla runbook-jobb eller visa detaljer om en viss runbook-jobb i hello Azure-portalen eller genom att konfigurera integrering med din jobbstatus för Microsoft Operations Management Suite (OMS) logganalys arbetsytan tooforward runbook och dataströmmar för jobbet.  Mer information om hur du integrerar med OMS Log Analytics finns [vidarebefordra jobbstatus och jobbet strömmar från Automation tooLog Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).  

### <a name="automation-runbook-jobs-summary"></a>Automation runbook-jobb sammanfattning
På hello höger i det valda Automation-kontot, visas en sammanfattning av alla hello runbook-jobb för ett valt Automation-konto under **jobbet statistik** panelen.<br><br> ![Jobbet statistik panelen](./media/automation-runbook-execution/automation-account-job-status-summary.png).<br> Den här panelen visar antalet och grafisk representation av hello jobbstatus för alla jobb som körs.  

Klicka på panelen hello visar hello **jobb** som innehåller en sammanfattande lista över alla jobb som körs, med status, jobbkörningen och start och slutförande gånger.<br><br> ![Jobb-bladet för Automation-konto](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)<br><br>  Du kan filtrera hello lista över jobb genom att välja **filtrera jobb** och filtrera efter en viss runbook jobbstatus, eller hello tidsvärdet intervallet toosearch inom hello nedrullningsbara listan.<br><br> ![Filtrera jobbstatus](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Du kan också du kan visa sammanfattningsinformation för jobbet för en viss runbook genom att välja den runbook hello **Runbooks** bladet i Automation-konto och välj sedan hello **jobb** panelen.  Detta innebär hello **jobb** bladet, och därifrån kan du klicka på hello jobbet poster tooview detaljer och effekt.<br><br> ![Jobb-bladet för Automation-konto](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)<br> 

### <a name="job-summary"></a>Jobbsammanfattning
Du kan visa en lista över alla hello jobb som har skapats för en viss runbook och deras senaste status. Du kan filtrera listan efter jobbstatus och hello datumintervall för hello senast ändrad toohello jobb. tooview dess detaljerad information och utdata, klicka på hello namn för ett jobb. hello innehåller detaljerad vy av hello jobbet hello värden för hello runbook-parametrar som angavs toothat jobb.

Du kan använda hello följande steg tooview hello jobb för en runbook.

1. Välj i hello Azure-portalen, **Automation** och välj sedan hello namnet på ett Automation-konto.
2. Hello hubben, Välj **Runbooks** och klicka sedan på hello **Runbooks** bladet Välj en runbook hello-listan.
3. Klicka på hello bladet för hello markerad runbook hello **jobb** panelen.
4. Klicka på en av hello jobb i hello lista på hello runbook-jobbet informationsbladet kan du visa detaljer och effekt.

## <a name="retrieving-job-status-using-windows-powershell"></a>Hämta jobbstatus med Windows PowerShell
Du kan använda hello [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) tooretrieve hello jobb som skapats för en runbook och hello information för ett specifikt jobb. Om du startar en runbook med Windows PowerShell med [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx), och sedan returnerar hello resulterande jobb. Använd [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx)utdata tooget utdata från ett jobb.

hello följande exempelkommandon hämtar hello senaste jobb för en exempel-runbook och visar dess status och hello värden för runbookparametrar hello hello utdata från hello jobb.

    $job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
    –RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
    $job.Status
    $job.JobParameters
    Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output

## <a name="fair-share"></a>Fördelning
I ordning tooshare resurser mellan alla runbooks i hello molnet inaktiveras Azure Automation tillfälligt jobb när den har körts under tre timmar.  Under denna tid jobb för [PowerShell-baserade runbooks](automation-runbook-types.md#powershell-runbooks) stoppas och startas inte.  Hej jobbet status visas **stoppad**.  Den här typen av runbook startas alltid från början hello eftersom de inte stöder kontrollpunkter.  

[PowerShell-Arbetsflödesbaserade runbooks](automation-runbook-types.md#powershell-workflow-runbooks) kommer att återupptas från deras senaste [kontrollpunkt](https://docs.microsoft.com/system-center/sma/overview-powershell-workflows#bk_Checkpoints).  När du har kört tre timmar hello runbook-jobbet kommer att inaktiveras hello service och dess status visas **körs, väntar på resurser**.  När en sandbox blir tillgänglig startas hello runbook automatiskt av hello Automation-tjänsten och återupptar från hello senaste kontrollpunkten.  Detta är normalt PowerShell-arbetsflöde att pausa/restart.  Om hello runbook igen överskrider tre timmars körning, upprepas hello in toothree gånger.  Efter hello tredje omstart, om hello runbook fortfarande inte har slutförts i tre timmar sedan hello runbook-jobbet misslyckades och visar hello jobbstatus **misslyckades, väntar på resurser**.  I det här fallet kan du få hello följande undantag med hello-fel.

*hello jobbet kan inte fortsätta eftersom det upprepade gånger har avlägsnats från hello samma kontrollpunkt. Kontrollera att din Runbook utför långvariga åtgärder utan att spara sitt tillstånd.*

Detta är tooprotect hello tjänst från runbooks som körs på obestämd tid utan att slutföra, eftersom de inte kan toomake den toohello nästa kontrollpunkt utan tas bort från minnet igen.

Om hello runbook har inga kontrollpunkter eller hello jobbet hade inte nått hello första kontrollpunkt innan tas bort från minnet, startas sedan om från början hello.  

När du skapar en runbook bör du kontrollera att hello tid toorun alla aktiviteter mellan två kontrollpunkter inte överskrider tre timmar. Du kan behöva tooadd kontrollpunkter tooyour runbook tooensure att den inte nå den här gränsen tre timmar eller dela upp långa kör åtgärder. Din runbook kan exempelvis utföra en omindexera på en stor SQL-databas. Om den här enda åtgärden inte slutförs inom hello verkliga resursen gränsen och sedan hello jobbet är minnet och starta om från början hello. I det här fallet bör du dela upp hello omindexera åtgärden i flera steg, till exempel startas en tabell i taget, och infoga en kontrollpunkt efter varje åtgärd så att hello jobbet återupptas efter hello senaste åtgärden toocomplete.

## <a name="next-steps"></a>Nästa steg
* toolearn mer om hello olika metoder som kan använda toostart en runbook i Azure Automation finns [starta en runbook i Azure Automation](automation-starting-a-runbook.md)

