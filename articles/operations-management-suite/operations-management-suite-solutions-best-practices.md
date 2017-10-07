---
title: "Metodtips för aaaOMSManagement lösning | Microsoft Docs"
description: 
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: 08cf1c101e301d24fb5c2bf4bc02a978e508a198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-creating-management-solutions-in-operations-management-suite-oms-preview"></a>Metodtips för att skapa lösningar för hantering i Operations Management Suite (OMS) (förhandsgranskning)
> [!NOTE]
> Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen. Ett schema som beskrivs nedan är ämne toochange.  

Den här artikeln innehåller metodtips för [att skapa en management lösningsfil](operations-management-suite-solutions-solution-file.md) i Operations Management Suite (OMS).  Den här informationen kommer att uppdateras när ytterligare metodtips identifieras.

## <a name="data-sources"></a>Datakällor
- Datakällor kan vara [konfigurerats med en Resource Manager-mall](../log-analytics/log-analytics-template-workspace-configuration.md), men de bör inte ingå i en lösningsfil.  hello beror på att konfigurera datakällor inte är för närvarande idempotent vilket innebär att din lösning kan du skriva över befintliga konfigurationen hello användarens arbetsytan.<br><br>Lösningen kan exempelvis kräva varnings- och händelser från hello programmets händelselogg.  Om du anger detta som en datakälla i din lösning, riskerar du att ta bort händelser med Information om hello användare har konfigurerat i arbetsytan.  Om du ingår alla händelser, det kan du samla överdriven informationshändelser i hello användarens arbetsyta.

- Om din lösning kräver data från en hello standard datakällor, bör du definiera detta som ett krav.  Tillstånd i dokumentationen hello kunden måste konfigurera hello datakälla på egen hand.  
- Lägg till en [flöda dataverifieringen](../log-analytics/log-analytics-view-designer-tiles.md) meddelandet tooany vyer i din lösning tooinstruct hello användare på datakällor som behöver toobe som konfigurerats för toobe nödvändiga data som samlas in.  Det här meddelandet visas på hello ikonen hello vyn när data som krävs saknas.


## <a name="runbooks"></a>Runbooks
- Lägg till en [Automation schema](../automation/automation-schedules.md) för varje runbook i din lösning som behöver toorun enligt ett schema.
- Inkludera hello [IngestionAPI modulen](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) i din lösning toobe som används av runbooks skriva data toohello logganalys-databasen.  Konfigurera hello lösning för[referens](operations-management-suite-solutions-solution-file.md#solution-resource) resursen så att den förblir om hello lösning har tagits bort.  Detta gör att flera lösningar tooshare hello modulen.
- Använd [automationsvariabler](../automation/automation-schedules.md) tooprovide värden toohello lösningen att användare kan behöva toochange senare.  Även om hello lösningen är konfigurerade toocontain hello variabel kan fortfarande så är dess värde ändras.

## <a name="views"></a>Vyer
- Alla lösningar ska innehålla en enda vy som visas i hello användarportalen.  hello vyn kan innehålla flera [visualiseringen delar](../log-analytics/log-analytics-view-designer-parts.md) tooillustrate olika anger av data.
- Lägg till en [flöda dataverifieringen](../log-analytics/log-analytics-view-designer-tiles.md) meddelandet tooany vyer i din lösning tooinstruct hello användare på datakällor som behöver toobe som konfigurerats för toobe nödvändiga data som samlas in.
- Konfigurera hello lösning för[innehåller](operations-management-suite-solutions-solution-file.md#solution-resource) hello vyn så att den tas bort om hello lösning har tagits bort.

## <a name="alerts"></a>Aviseringar
- Definiera hello mottagare som en parameter i hello lösningsfilen så hello användare kan definiera dem när de installerar hello lösning.
- Konfigurera hello lösning för[referens](operations-management-suite-solutions-solution-file.md#solution-resource) avisering regler så att användaren kan ändra sin konfiguration.  De kanske toomake ändringar, till exempel ändra lista över mottagare hello, ändra hello avisering hello tröskelvärdet eller inaktivera hello varningsregel. 


## <a name="next-steps"></a>Nästa steg
* Gå igenom hello grundläggande processen för [designa och skapa en lösning för](operations-management-suite-solutions-creating.md).
* Lär dig hur för[skapa en lösningsfil](operations-management-suite-solutions-solution-file.md).
* [Lägg till sparade sökningar och aviseringar](operations-management-suite-solutions-resources-searches-alerts.md) tooyour hanteringslösning.
* [Lägga till vyer](operations-management-suite-solutions-resources-views.md) tooyour hanteringslösning.
* [Lägg till Automation-runbooks och andra resurser](operations-management-suite-solutions-resources-automation.md) tooyour hanteringslösning.

