---
title: aaaUpdate Azure moduler i Azure Automation | Microsoft Docs
description: "Den här artikeln beskriver hur du kan nu uppdatera vanliga Azure PowerShell-moduler som tillhandahålls som standard i Azure Automation."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2017
ms.author: magoedte
ms.openlocfilehash: afa404a643349f044e55be2280e435b575049dec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-azure-powershell-modules-in-azure-automation"></a>Hur tooupdate Azure PowerShell-moduler i Azure Automation

hello vanligaste Azure PowerShell-moduler som tillhandahålls som standard i varje Automation-kontot.  hello Azure-teamet uppdateringar hello Azure moduler regelbundet, så i hello Automation-konto får ett sätt som du tooupdate hello moduler i hello-konto när nya versioner är tillgängliga från hello-portalen.  

Eftersom moduler uppdateras regelbundet med hello produktgruppen, kan förändringar uppstå med hello ingår cmdlet: ar, vilket kan påverka dina runbooks beroende på hello typ av ändring, till exempel byta namn på en parameter eller sluta en cmdlet helt. tooavoid påverkar dina runbooks och hello de automatisera processer, rekommenderas du att testa och verifiera innan du fortsätter.  Om du inte har ett särskilt Automation-konto som är avsedda för detta ändamål kan du skapa en så att du kan testa många olika scenarier och permutationer under hello utvecklingen av dina runbooks i tillägget tooiterative ändras, som uppdatering hello PowerShell-moduler.  När hello resultat verifieras och du har gjort några ändringar krävs, fortsätta med samordning hello migrering av alla runbooks som krävs för ändring och utföra hello uppdatering som beskrivs nedan i produktion.     

## <a name="updating-azure-modules"></a>Uppdatera Azure moduler

1. I hello moduler bladet för Automation-kontot det är ett alternativ som kallas **Update Azure moduler**.  Det är alltid aktiverat.<br><br> ![Uppdatera Azure moduler alternativ i bladet för moduler](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

2. Klicka på **Update Azure moduler** och du kommer att visas ett meddelande om bekräftelse som frågar om du vill toocontinue.<br><br> ![Uppdatera Azure moduler meddelande](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Klicka på **Ja** och hello modulen uppdateringen påbörjas.  hello uppdateringen tar 15-20 minuter tooupdate hello följande moduler:

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Om hello moduler är redan igång toodate, slutförs hello processen inom några sekunder.  När hello uppdateringen är klar meddelas du.<br><br> ![Uppdatera uppdateringsstatus för Azure-moduler](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> Azure Automation använder hello senaste moduler i ditt Automation-konto när ett nytt schemalagt jobb körs.    

Om du använder cmdlets från dessa Azure PowerShell-moduler i dina runbooks toomanage Azure resurser, kommer du tooperform uppdateringsprocessen varje månad eller så tooassure som du har hello senaste moduler.

## <a name="next-steps"></a>Nästa steg

* toolearn mer om integreringsmoduler och hur toocreate anpassade moduler toofurther integrera Automation med andra system, tjänster eller lösningar, se [integreringsmoduler](automation-integration-modules.md).

* Överväg källa kontrollen integrering med [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) eller [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) toocentrally hantera och styra versioner av ditt Automation-runbook och konfiguration portfölj.  
