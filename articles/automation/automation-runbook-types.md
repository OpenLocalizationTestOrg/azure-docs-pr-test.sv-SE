---
title: aaaAzure Automation Runbook-typer | Microsoft Docs
description: "Beskriver hello olika typer av runbooks som du kan använda i Azure Automation- och säkerhetsaspekter som du bör beakta när du fastställer vilken typ av toouse. "
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9265c975-4281-4819-a84f-d86641277f36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/01/2017
ms.author: bwren
ms.openlocfilehash: c28aa57c77025764b16784372308a4ff2f596914
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-runbook-types"></a>Typer av Azure Automation-runbook
Azure Automation stöder fyra typer av runbooks som beskrivs kortfattat i hello i den följande tabellen.  hello avsnitt nedan innehåller ytterligare information om varje typ av bland annat tänka på när toouse varje.

| Typ | Beskrivning |
|:--- |:--- |
| [Grafisk](#graphical-runbooks) |Baserat på Windows PowerShell och skapas och redigerade helt i grafiska redigerare i Azure-portalen. |
| [Grafisk PowerShell-arbetsflöde](#graphical-runbooks) |Baserat på Windows PowerShell-arbetsflöde och skapas och redigerade helt i hello grafiska redigerare i Azure-portalen. |
| [PowerShell](#powershell-runbooks) |Text-runbook som baseras på Windows PowerShell-skript. |
| [PowerShell-arbetsflöde](#powershell-workflow-runbooks) |Text-runbook som baseras på Windows PowerShell-arbetsflöde. |

## <a name="graphical-runbooks"></a>Grafiska runbook-flöden
[Grafisk](automation-runbook-types.md#graphical-runbooks) och grafisk PowerShell-arbetsflöde runbooks skapas och redigeras med hello grafiska redigerare i hello Azure-portalen.  Du kan exportera dem tooa fil och importera dem till en annan automation-konto, men du kan inte skapa eller redigera dem med ett annat verktyg.  Grafiska runbook-flöden generera PowerShell-kod, men du inte visa eller ändra hello koden direkt. Grafiska runbook-flöden går inte att konvertera tooone av hello [textformat](automation-runbook-types.md), eller kan vara en text-runbook konverterade toographical format. Grafiska runbook-flöden kan vara konverterade tooGraphical runbooks med PowerShell-arbetsflöde vid import och vice versa.

### <a name="advantages"></a>Fördelar
* Visual insert-länk-konfigurera redigering modellen  
* Fokusera på hur data flödar genom hello-processen  
* Visuellt representera hanteringsprocesser  
* Med andra runbooks som underordnade runbooks toocreate hög nivå arbetsflöden  
* Uppmuntrar modulära programmering  


### <a name="limitations"></a>Begränsningar
* Det går inte att redigera runbook utanför Azure-portalen.
* Kan kräva en kod aktivitet som innehåller PowerShell kod tooperform komplex logik.
* Det går inte att visa eller redigera direkt hello PowerShell-koden som skapas av hello grafiskt arbetsflöde. Observera att du kan visa hello-kod som du skapar i koden aktiviteter.

## <a name="powershell-runbooks"></a>PowerShell-runbooks
PowerShell-runbooks är baserade på Windows PowerShell.  Du redigerar direkt hello koden för hello runbook med hello textredigerare i hello Azure-portalen.  Du kan också använda en textredigerare som offline och [importera hello runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) i Azure Automation.

### <a name="advantages"></a>Fördelar
* Implementera alla komplex logik med PowerShell-koden utan hello ytterligare svårigheter med PowerShell-arbetsflöde. 
* Runbook startar snabbare än PowerShell-arbetsflöde runbooks eftersom det inte behöver toobe kompileras innan du kör.

### <a name="limitations"></a>Begränsningar
* Måste vara bekant med PowerShell-skript.
* Det går inte att använda [parallellbearbetning](automation-powershell-workflow.md#parallel-processing) tooperform flera åtgärder parallellt.
* Det går inte att använda [kontrollpunkter](automation-powershell-workflow.md#checkpoints) tooresume runbook vid fel.
* PowerShell-arbetsflöde runbooks och grafiska runbook-flöden kan endast inkluderas som underordnade runbooks med cmdleten hello Start AzureAutomationRunbook som skapar ett nytt jobb.

### <a name="known-issues"></a>Kända problem
Följande är aktuella kända problem med PowerShell-runbooks.

* PowerShell-runbooks kan inte det går inte att hämta en okrypterad [variabeltillgång](automation-variables.md) med ett null-värde.
* PowerShell-runbooks kan inte hämta en [variabeltillgång](automation-variables.md) med  *~*  i hello namn.
* Get-Process i en loop i ett PowerShell-runbook kan krascha efter ca 80 iterationer. 
* En PowerShell-runbook kan misslyckas om den försöker toowrite en mycket stor mängd data toohello utdataströmmen samtidigt.   Du kan ofta undvika det här problemet genom att skicka ut precis hello information du behöver när du arbetar med stora objekt.  Till exempel i stället för att mata ut ungefär så *Get-Process*, du kan spara bara hello krävs fält med *Get-Process | Välj processnamn CPU*.

## <a name="powershell-workflow-runbooks"></a>PowerShell-arbetsflöde runbooks
PowerShell-arbetsflöde runbooks är text runbooks enligt [Windows PowerShell-arbetsflöde](automation-powershell-workflow.md).  Du redigerar direkt hello koden för hello runbook med hello textredigerare i hello Azure-portalen.  Du kan också använda en textredigerare som offline och [importera hello runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) i Azure Automation.

### <a name="advantages"></a>Fördelar
* Implementera alla komplex logik med PowerShell-arbetsflöde kod.
* Använd [kontrollpunkter](automation-powershell-workflow.md#checkpoints) tooresume runbook vid fel.
* Använd [parallellbearbetning](automation-powershell-workflow.md#parallel-processing) tooperform flera åtgärder parallellt.
* Kan innehålla andra grafiska runbook-flöden och PowerShell-arbetsflöde runbooks som underordnade runbooks toocreate hög nivå arbetsflöden.

### <a name="limitations"></a>Begränsningar
* Författare måste vara bekant med PowerShell-arbetsflöde.
* Runbook måste hantera hello ytterligare komplexitet med PowerShell-arbetsflöde som [avserialiseras objekt](automation-powershell-workflow.md#code-changes).
* Runbook tar längre tid toostart än PowerShell runbooks eftersom den måste kompileras innan du kör toobe.
* PowerShell-runbooks kan bara inkluderas som underordnade runbooks med cmdleten hello Start AzureAutomationRunbook som skapar ett nytt jobb.

## <a name="considerations"></a>Överväganden
Du bör ta hänsyn hello följande ytterligare överväganden när du fastställer vilken typ av toouse för en viss runbook.

* Du kan inte konvertera runbooks från grafiska tootextual typ eller vice versa.
* Det finns begränsningar med hjälp av runbooks med olika typer som en underordnad runbook.  Se [underordnade runbooks i Azure Automation](automation-child-runbooks.md) för mer information.

## <a name="next-steps"></a>Nästa steg
* toolearn mer information om redigering av grafisk runbook finns [grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md)
* toounderstand hello skillnaderna mellan PowerShell och PowerShell arbetsflöden för runbooks, se [Learning Windows PowerShell-arbetsflöde](automation-powershell-workflow.md)
* Mer information om hur toocreate eller importera en Runbook finns [skapa eller importera en Runbook](automation-creating-importing-runbook.md)

