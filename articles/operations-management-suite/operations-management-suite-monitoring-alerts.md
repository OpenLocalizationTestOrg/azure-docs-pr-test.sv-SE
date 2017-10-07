---
title: aaaAlert hantering i Microsoft monitoring produkter | Microsoft Docs
description: "En avisering anger vissa problem som kräver uppmärksamhet från en administratör.  Den här artikeln beskriver hello skillnaderna i hur aviseringar skapas och hanteras i System Center Operations Manager (SCOM) och logganalys och ger bästa praxis i utnyttja hello två produkter för en strategi för avisering hybrid."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6572c3f8-78ca-4fa9-8fe1-d0b488590788
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 526a3d92f3b6a5d6dec2f3979a934cc94c13f2e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-alerts-with-microsoft-monitoring"></a>Hantera aviseringar med övervakning av Microsoft
En avisering anger vissa problem som kräver uppmärksamhet från en administratör.  Det finns specifika skillnader mellan System Center Operations Manager (SCOM) och logganalys i Operations Management Suite (OMS) när det gäller hur aviseringar skapas, hur de hanteras och analyseras och hur du meddelas att ett allvarligt problem har upptäckts.

## <a name="alerts-in-operations-manager"></a>Varningar i Operations Manager
Aviseringar i SCOM genereras av enskilda regler eller Övervakare tooindicate ett specifikt problem.  En Övervakare kan generera en avisering när den försätts i ett feltillstånd när en regel kan generera en avisering tooindicate vissa viktiga problem som inte är direkt relaterade toohello hälsotillståndet för ett hanterat objekt.  Hanteringspaket omfattar en mängd olika arbetsflöden som skapar aviseringarna för hello program eller tjänst som de hanterar.  En del av hello konfigureringen av ett nytt management pack finjustering den tooensure att du inte får mycket aviseringar för problem som du inte anser kritiska.

![SCOM Aviseringsvy](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM ger fullständig aviseringshanteringen aviseringar som har en status som kan ändras av administratörer när de arbetar tooresolve hello problemet.  Hello administratören definierar hello avisering tooclosed då visas den inte längre i vyer som visar aktiva aviseringar när hello problemet har lösts.  Aviseringar som genereras av Övervakare kan lösas automatiskt när övervakaren hello returnerar tooa felfritt tillstånd.

![Aviseringsstatus](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Aviseringar i logganalys
En avisering i logganalys skapas från en logg-fråga som körs automatiskt med jämna mellanrum.  Du kan skapa en aviseringsregel från alla log-fråga.  Om hello frågan returnerar resultat som matchar hello kriterier som du anger, skapas en avisering.  Detta kan vara en specifik fråga som skapar en avisering om en viss händelse identifieras eller du kan använda en mer allmän fråga som söker efter en felhändelse relaterad tooa visst program.

Log Analytics aviseringar toohello OMS-databasen som en händelse skrivs och kan hämtas med en fråga som loggen.  De har inte statusen som SCOM händelser så att du kan ange när hello problemet har lösts.

![OMS-avisering](media/operations-management-suite-monitoring-alerts/oms-alert.png)

När SCOM används som datakälla för Log Analytics, skrivs toohello OMS databasen SCOM aviseringar som skapas och ändras.  

![SCOM-avisering](media/operations-management-suite-monitoring-alerts/scom-alert.png)

Hej [avisering hanteringslösning](http://technet.microsoft.com/library/mt484092.aspx) innehåller en översikt över aktiva aviseringar och flera vanliga frågor tooretrieve olika uppsättningar av aviseringar.  Detta ger dig effektivare analys av aviseringarna än en rapport i SCOM.  Kan du detaljgranska från hello sammanfattningar toodetailed data och skapa ad hoc-frågor tooretrieve olika uppsättningar av aviseringar.

![Lösning för avisering](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Meddelanden
Meddelanden i SCOM skickar du ett e-post eller en text i svaret tooalerts som uppfyller specifika villkor.  Du kan skapa olika meddelandeprenumerationer som har olika personer meddelande beroende på dessa kriterier som hello-objekt som övervakas, hello allvarlighetsgraden hello avisering, hello typen av problem som identifieras eller hello tid på dagen.

Några prenumerationer kan vara används tooimplement en fullständig anmälan strategi för ett stort antal hanteringspaket.

![SCOM aviseringar](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Logganalys kan meddela dig via e-post att en avisering har skapats genom att ange en e-postavisering åtgärd på varje [varningsregeln](http://technet.microsoft.com/library/mt614775.aspx).  Hello möjlighet för SCOM hello prenumerera toomultiple aviseringar med en enda regel finns inte.  Du måste också toocreate egna Varningsregler eftersom OMS inte tillhandahåller någon förinställda.

![Log Analytics-aviseringar](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Du kan inte helt hantera SCOM aviseringar i logganalys men eftersom du kan bara ändra dem i hello Operations-konsolen.  Logganalys är användbar som en del av en aviseringshanteringen bearbeta om för att tillhandahålla analysverktyg som enbart SCOM saknar.

## <a name="alert-remediation"></a>Aviseringen reparation
[Reparation](http://technet.microsoft.com/library/mt614775.aspx) refererar tooan försök tooautomatically rätt hello problem som identifieras av en avisering.

SCOM kan du toorun diagnostik och återhämtning i svaret tooa Övervakaren för att ange ett giltigt tillstånd.  Detta sker samtidigt toohello Övervakare skapar hello avisering.  Diagnostik och återställningar implementeras normalt som ett skript som körs på hello-agenten.  En diagnostisk försök toogather mer information om hello upptäcktes problem när en återställning försöker toocorrect hello problem.

Log Analytics kan du toostart en [Azure Automation-runbook](https://azure.microsoft.com/documentation/services/automation/) eller anropa en webhook i svaret tooa logganalys avisering.  Runbooks kan innehålla avancerad logik som implementerats i PowerShell.  hello skript körs i Azure och kan komma åt Azure-resurser eller externa resurser som är tillgängliga från hello molnet.  Azure Automation har hello möjlighet tooexecute runbooks på en server i ditt lokala datacenter, men den här funktionen är inte tillgänglig när du startar hello runbook i svaret tooLog Analytics aviseringar.

Både återställningar i SCOM och runbooks i OMS kan innehålla PowerShell-skript, men återställningar är svårare toocreate och hantera eftersom de måste finnas i ett hanteringspaket.  Runbooks som lagras i Azure Automation som innehåller funktioner för redigering, testning och hantering av runbooks.

Om du använder SCOM som en datakälla för Log Analytics kan du skapa en Log Analytics-avisering som använder en logg frågan tooretrieve SCOM aviseringar lagras i hello OMS-databasen.  Det innebär att toorun en Azure Automation-runbook i svaret tooa SCOM avisering.  Naturligtvis eftersom hello runbook ska köras i Azure, kan detta inte en lönsam strategi för återställningar av lokala problem.

## <a name="next-steps"></a>Nästa steg
* Läs hello information om [aviseringar i System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).

