---
title: "aaaIntegrate Azure Automation med Visual Stuido Team Services källkontrollen | Microsoft Docs"
description: "Scenariot beskriver hur du ställer in integration med en Azure Automation-konto och Visual Stuido Team Services källkontroll."
services: automation
documentationcenter: 
author: eamono
manager: 
editor: 
keywords: "Azure powershell, VSTS, källkontroll, automation"
ms.assetid: a43b395a-e740-41a3-ae62-40eac9d0ec00
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.openlocfilehash: 8f6faa596a5ad1f8b72e820ca320b3e103d83579
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a>Azure Automation-scenario – Automation källkontrollintegrering med Visual Studio Team Services

I det här scenariot har du ett Visual Studio Team Services-projekt som du använder toomanage Azure Automation-runbook eller DSC-konfigurationer för källkontroll.
Den här artikeln beskriver hur toointegrate VSTS med Azure Automation-miljön så att kontinuerlig integration sker för varje incheckning.

## <a name="getting-hello-scenario"></a>Hämta hello scenario

Det här scenariot består av två PowerShell-runbooks som kan importeras direkt från hello [Runbook-galleriet](automation-runbook-gallery.md) hello Azure-portalen eller ladda ned från hello [PowerShell-galleriet](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | Beskrivning| 
--------|------------|
Synkronisera VSTS | Importera runbooks eller konfigurationer från VSTS källkontroll när en incheckning är klar. Om du kör manuellt den importera och publicera alla runbooks eller konfigurationer i hello Automation-konto.| 
Synkronisera VSTSGit | Importera runbooks eller konfigurationer från VSTS Git källkontroll när en incheckning är klar. Om du kör manuellt den importera och publicera alla runbooks eller konfigurationer i hello Automation-konto.|

### <a name="variables"></a>Variabler

Variabel | Beskrivning|
-----------|------------|
VSToken | Säker variabeltillgång skapar du som innehåller hello VSTS personliga åtkomsttoken. Du kan lära dig hur toocreate en VSTS personlig åtkomst-token på hello [VSTS autentiseringssidan](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview). 
## <a name="installing-and-configuring-this-scenario"></a>Installera och konfigurera det här scenariot

Skapa en [personlig åtkomsttoken](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) i VSTS som du ska använda toosync hello runbooks eller konfigurationer till ditt automation-konto.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Skapa en [säker variabeln](automation-variables.md) i ditt automation-konto toohold hello personliga token så att hello runbook kan autentisera tooVSTS och sync hello runbooks eller konfigurationer till hello Automation-konto. Du kan kalla den här VSToken. 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Importera hello-runbook som kommer att synkronisera dina runbooks eller konfigurationer i hello automation-konto. Du kan använda hello [VSTS exempel-runbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) eller hello [VSTS med Git exempel-runbook] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) från hello PowerShellGallery.com beroende på om du använder VSTS käll-kontroll eller VSTS med Git och distribuera tooyour automation-konto.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Du kan nu [publicera](automation-creating-importing-runbook.md#publishing-a-runbook) denna runbook så att du kan skapa en webhook. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Skapa en [webhook](automation-webhooks.md) för synkronisering VSTS runbook och Fyll i hello parametrar som visas nedan. Kontrollera att du kopierar hello webhooksadressen som du behöver för en tjänst hook i VSTS. Hej VSAccessTokenVariableName är hello namn (VSToken) för hello säker variabel som du skapade tidigare toohold hello personliga åtkomsttoken. 

Integrera med VSTS (synkronisera VSTS.ps1) tar hello följande parametrar.
### <a name="sync-vsts-parameters"></a>Synkronisera VSTS parametrar

Parameter | Beskrivning| 
--------|------------|
WebhookData | Detta kommer att innehålla hello checka in informationen som skickas från hello VSTS service hook. Den här parametern bör lämna tomt.| 
ResourceGroup | Det här är hello hello resursgruppen som hello automation-konto.|
AutomationAccountName | hello namnet på hello automation-konto som synkroniseras med VSTS.|
VSFolder | Namnet på mappen hello i VSTS där hello runbooks och konfigurationer finns.|
VSAccount | hello namnet på hello Visual Studio Team Services-konto.| 
VSAccessTokenVariableName | hello namnet på hello säker variabeln (VSToken) som innehåller hello VSTS personliga åtkomsttoken.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Om du använder VSTS med GIT (synkronisera VSTSGit.ps1) tar hello följande parametrar.

Parameter | Beskrivning|
--------|------------|
WebhookData | Detta kommer att innehålla hello checka in informationen som skickas från hello VSTS service hook. Den här parametern bör lämna tomt.| ResourceGroup | Den här hello namnet på hello resursgrupp som hello automation-konto.|
AutomationAccountName | hello namnet på hello automation-konto som synkroniseras med VSTS.|
VSAccount | hello namnet på hello Visual Studio Team Services-konto.|
VSProject | hello namnet på hello-projekt i VSTS där hello runbooks och konfigurationer finns.|
GitRepo | hello namnet på hello Git-lagringsplats.|
GitBranch | hello namnet på hello gren i VSTS Git-lagringsplats.|
Mapp | hello namn på hello mapp i VSTS Git grenen.|
VSAccessTokenVariableName | hello namnet på hello säker variabeln (VSToken) som innehåller hello VSTS personliga åtkomsttoken.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Skapa en tjänst hook i VSTS för incheckningar toohello mapp som utlöser denna webhook på Checka in kod. Välj webbserver skapar som hello service toointegrate med när du skapar en ny prenumeration. Du kan lära dig mer om tjänsten hook på [VSTS Service hook dokumentationen](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Du bör nu kunna toodo alla incheckningar runbooks och konfigurationer i VSTS och dessa automatiskt synkroniseras hade till ditt automation-konto.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Om du kör runbook manuellt utan som utlöses av VSTS, kan du låta hello webhookdata parametern tom och den kommer att göra en fullständig synkronisering från hello VSTS mappen som anges.

Om du inte vill toouninstall hello scenariot, ta bort hello service hook från VSTS, ta bort hello runbook och hello VSToken variabeln.
