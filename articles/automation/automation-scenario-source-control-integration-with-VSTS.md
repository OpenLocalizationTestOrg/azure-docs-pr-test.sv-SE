---
title: "Integrera Azure Automation med Visual Stuido Team Services källkontrollen | Microsoft Docs"
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
ms.openlocfilehash: 01f9c01c9e04e02dbb548b68cf99684ba6ddd57e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a>Azure Automation-scenario – Automation källkontrollintegrering med Visual Studio Team Services

I det här scenariot har du ett projekt i Visual Studio Team Services som du använder för att hantera Azure Automation-runbooks eller DSC-konfigurationer för källkontroll.
Den här artikeln beskriver hur du integrerar VSTS med Azure Automation-miljö så att kontinuerlig integration sker för varje incheckning.

## <a name="getting-the-scenario"></a>Hämta scenariot

Det här scenariot består av två PowerShell-runbooks som kan importeras direkt från den [Runbook-galleriet](automation-runbook-gallery.md) i Azure-portalen eller hämtas från den [PowerShell-galleriet](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | Beskrivning| 
--------|------------|
Synkronisera VSTS | Importera runbooks eller konfigurationer från VSTS källkontroll när en incheckning är klar. Om du kör manuellt den importera och publicera alla runbooks eller konfigurationer i Automation-kontot.| 
Synkronisera VSTSGit | Importera runbooks eller konfigurationer från VSTS Git källkontroll när en incheckning är klar. Om du kör manuellt den importera och publicera alla runbooks eller konfigurationer i Automation-kontot.|

### <a name="variables"></a>Variabler

Variabel | Beskrivning|
-----------|------------|
VSToken | Säker variabeltillgång skapar du som innehåller VSTS personliga åtkomsttoken. Du kan lära dig hur du skapar en personlig åtkomsttoken VSTS på den [VSTS autentiseringssidan](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview). 
## <a name="installing-and-configuring-this-scenario"></a>Installera och konfigurera det här scenariot

Skapa en [personlig åtkomsttoken](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) i VSTS som du använder för att synkronisera runbooks eller konfigurationer i ditt automation-konto.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Skapa en [säker variabeln](automation-variables.md) i ditt automation-konto för att rymma personlig åtkomst-token så att runbook kan autentisera till VSTS och synkronisera runbooks eller konfigurationer i Automation-kontot. Du kan kalla den här VSToken. 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Importera runbook som kommer att synkronisera dina runbooks eller konfigurationer till automation-kontot. Du kan använda den [VSTS exempel-runbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) eller [VSTS med Git exempel-runbook] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) från PowerShellGallery.com beroende på om du använder VSTS källa kontrollen eller VSTS med Git och distribuera dem till ditt automation-konto.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Du kan nu [publicera](automation-creating-importing-runbook.md#publishing-a-runbook) denna runbook så att du kan skapa en webhook. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Skapa en [webhook](automation-webhooks.md) för synkronisering VSTS runbook och Fyll i parametrarna som visas nedan. Kontrollera att du kopierar webhooksadressen som du behöver för en tjänst hook i VSTS. VSAccessTokenVariableName är namn (VSToken) för säker variabeln som du skapade tidigare för att rymma den personliga åtkomsttoken. 

Integrera med VSTS (synkronisera VSTS.ps1) tar följande parametrar.
### <a name="sync-vsts-parameters"></a>Synkronisera VSTS parametrar

Parameter | Beskrivning| 
--------|------------|
WebhookData | Checka in informationen som skickas från VSTS service hook kommer att innehålla. Den här parametern bör lämna tomt.| 
ResourceGroup | Detta är namnet på resursgruppen som automation-kontot.|
AutomationAccountName | Namnet på automation-kontot kommer att synkroniseras med VSTS.|
VSFolder | Namnet på mappen i VSTS där runbooks och konfigurationer finns.|
VSAccount | Namnet på Visual Studio Team Services-konto.| 
VSAccessTokenVariableName | Namnet på den säkra variabel (VSToken) som innehåller VSTS personliga åtkomsttoken.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Om du använder VSTS med GIT (synkronisera VSTSGit.ps1) tar följande parametrar.

Parameter | Beskrivning|
--------|------------|
WebhookData | Checka in informationen som skickas från VSTS service hook kommer att innehålla. Den här parametern bör lämna tomt.| ResourceGroup | Det här namnet på resursgruppen som automation-kontot.|
AutomationAccountName | Namnet på automation-kontot kommer att synkroniseras med VSTS.|
VSAccount | Namnet på Visual Studio Team Services-konto.|
VSProject | Namnet på projektet i VSTS där runbooks och konfigurationer finns.|
GitRepo | Namnet på Git-lagringsplats.|
GitBranch | Namnet på filialen i VSTS Git-lagringsplats.|
Mapp | Namnet på mappen i VSTS Git grenen.|
VSAccessTokenVariableName | Namnet på den säkra variabel (VSToken) som innehåller VSTS personliga åtkomsttoken.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Skapa en tjänst hook i VSTS för incheckningar till den mapp som utlöser denna webhook på Checka in kod. Välj webbserver skapar som tjänsten för att integrera med när du skapar en ny prenumeration. Du kan lära dig mer om tjänsten hook på [VSTS Service hook dokumentationen](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Du bör nu kunna göra alla incheckningar runbooks och konfigurationer i VSTS och låta dessa automatiskt synkronisering d i ditt automation-konto.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Om du kör runbook manuellt utan som utlöses av VSTS parametern webhookdata du kan lämna tomt och den kommer att göra en fullständig synkronisering från mappen VSTS anges.

Om du vill avinstallera scenariot, ta bort tjänsten hook från VSTS bort runbook och variabeln VSToken.
