---
title: aaaAutomate borttagning av resursgrupper | Microsoft Docs
description: "PowerShell-arbetsflöde version av ett Azure Automation-scenario inklusive runbooks tooremove alla resursgrupper i din prenumeration."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: 
ms.assetid: b848e345-fd5d-4b9d-bc57-3fe41d2ddb5c
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/26/2016
ms.author: magoedte
ms.openlocfilehash: d7ff8064842385d57b0eebdf7b263150c958255f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---automate-removal-of-resource-groups"></a>Azure Automation-scenario – automatiserad borttagning av resursgrupper
Många kunder skapar fler än en resursgrupp. En del används för att hantera driftsprogram, andra används som utvecklings-, testnings-, eller mellanlagringsmiljöer. Automatisera hello distributionen av dessa resurser är en sak, men som kan toodecommission en resursgrupp med en hello knappen är en annan. Du kan förenkla den här vanliga hanteringsåtgärden med hjälp av Azure Automation. Det här är användbart om du arbetar med en Azure-prenumeration som har en utgiftsgräns via en medlemserbjudandet som MSDN eller hello Microsoft Partner Network Cloud Essentials-programmet.

Det här scenariot är baserad på en PowerShell-runbook och är utformad tooremove en eller flera resursgrupper som du anger från prenumerationen. hello standardinställningen för hello runbook är tootest innan du fortsätter. Detta säkerställer att du inte av misstag tar bort hello resursgrupp innan du är klar toocomplete den här proceduren.   

## <a name="getting-hello-scenario"></a>Hämta hello scenario
Det här scenariot består av en PowerShell-runbook som du kan hämta från hello [PowerShell-galleriet](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript). Du kan också importera den direkt från hello [Runbook-galleriet](automation-runbook-gallery.md) i hello Azure-portalen.<br><br>

| Runbook | Beskrivning |
| --- | --- |
| Remove-ResourceGroup |Tar bort en eller flera Azure-resursgrupper och associerade resurser från hello prenumeration. |

<br>
hello följande indataparametrar har angetts för denna runbook:

| Parameter | Beskrivning |
| --- | --- |
| NameFilter (obligatorisk) |Anger ett namn filter toolimit hello resursgrupper som du tänkt på Ta bort. Du kan skicka flera värden med hjälp av en kommaavgränsad lista.<br>hello-filtret är inte skiftlägeskänsligt och matchar valfri resursgrupp som innehåller hello-sträng. |
| PreviewMode (valfritt) |Kör hello runbook toosee vilka resursgrupper skulle tas bort, men ingen åtgärd vidtas.<br>hello standardvärdet är **SANT** toohelp undvika oavsiktlig borttagning av en eller flera resursgrupper skickades toohello runbook. |

## <a name="install-and-configure-this-scenario"></a>Installera och konfigurera det här scenariot
### <a name="prerequisites"></a>Krav
Denna runbook autentiserar med hjälp av hello [Azure kör som-konto](automation-sec-configure-azure-runas-account.md).    

### <a name="install-and-publish-hello-runbooks"></a>Installera och publicera hello runbooks
När du har hämtat hello runbook kan du importera det med hello proceduren i [importera runbook procedurer](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation). Publicera hello runbook när den har importerats till ditt Automation-konto.

## <a name="using-hello-runbook"></a>Med hjälp av hello runbook
hello vägleder följande steg dig igenom hello körningen av den här runbook och hjälp du bekanta dig med hur det fungerar. Du ska bara testa hello runbook i det här exemplet inte att ta bort hello resursgruppen.  

1. Hello Azure-portalen, öppna ditt Automation-konto och klicka på **Runbooks**.
2. Välj hello **ta bort ResourceGroup** runbook och klicka på **starta**.
3. När du startar hello runbook hello **starta Runbook** blad öppnas och du kan konfigurera hello parametrar. Ange hello namnen på resursgrupper i din prenumeration som du kan använda för att testa och gör ingen skada om bort av misstag.<br> ![Parametrar för Remove-ResouceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-input-parameters.png)

   > [!NOTE]
   > Kontrollera att **Previewmode** har angetts för**SANT** tooavoid bort hello valt resursgrupper.  **Obs** att denna runbook inte tar bort hello resursgruppen som innehåller hello Automation-konto med denna runbook.  
   >
   >
4. När du har konfigurerat alla hello parametervärden, klickar du på **OK**, och hello runbook placeras i kö för körning.  

tooview hello information om hello **ta bort ResourceGroup** runbook-jobb i hello Azure portal, Välj **jobb** i hello runbook. hello jobbet sammanfattning visar hello indataparametrar och hello utdata strömma dessutom toogeneral information om hello jobbet och eventuella undantag som uppstått.<br> ![Remove-ResourceGroup runbook jobbstatus](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

Hej **jobbsammanfattning** innehåller meddelanden från hello utdata, varningar och fel dataströmmar. Välj **utdata** tooview detaljerade resultat från hello runbook-körningen.<br> ![Remove-ResourceGroup runbook utdataresultat](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Nästa steg
* tooget igång med att skapa egna runbook finns [skapa eller importera en runbook i Azure Automation](automation-creating-importing-runbook.md).
* tooget igång med PowerShell-arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md).
