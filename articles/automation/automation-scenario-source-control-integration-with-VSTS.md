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
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a><span data-ttu-id="00ccf-104">Azure Automation-scenario – Automation källkontrollintegrering med Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="00ccf-104">Azure Automation scenario - Automation source control integration with Visual Studio Team Services</span></span>

<span data-ttu-id="00ccf-105">I det här scenariot har du ett projekt i Visual Studio Team Services som du använder för att hantera Azure Automation-runbooks eller DSC-konfigurationer för källkontroll.</span><span class="sxs-lookup"><span data-stu-id="00ccf-105">In this scenario, you have a Visual Studio Team Services project that you are using to manage Azure Automation runbooks or DSC configurations under source control.</span></span>
<span data-ttu-id="00ccf-106">Den här artikeln beskriver hur du integrerar VSTS med Azure Automation-miljö så att kontinuerlig integration sker för varje incheckning.</span><span class="sxs-lookup"><span data-stu-id="00ccf-106">This article describes how to integrate VSTS with your Azure Automation environment so that continuous integration happens for each check-in.</span></span>

## <a name="getting-the-scenario"></a><span data-ttu-id="00ccf-107">Hämta scenariot</span><span class="sxs-lookup"><span data-stu-id="00ccf-107">Getting the scenario</span></span>

<span data-ttu-id="00ccf-108">Det här scenariot består av två PowerShell-runbooks som kan importeras direkt från den [Runbook-galleriet](automation-runbook-gallery.md) i Azure-portalen eller hämtas från den [PowerShell-galleriet](https://www.powershellgallery.com).</span><span class="sxs-lookup"><span data-stu-id="00ccf-108">This scenario consists of two PowerShell runbooks that you can import directly from the [Runbook Gallery](automation-runbook-gallery.md) in the Azure portal or download from the [PowerShell Gallery](https://www.powershellgallery.com).</span></span>

### <a name="runbooks"></a><span data-ttu-id="00ccf-109">Runbooks</span><span class="sxs-lookup"><span data-stu-id="00ccf-109">Runbooks</span></span>

<span data-ttu-id="00ccf-110">Runbook</span><span class="sxs-lookup"><span data-stu-id="00ccf-110">Runbook</span></span> | <span data-ttu-id="00ccf-111">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="00ccf-111">Description</span></span>| 
--------|------------|
<span data-ttu-id="00ccf-112">Synkronisera VSTS</span><span class="sxs-lookup"><span data-stu-id="00ccf-112">Sync-VSTS</span></span> | <span data-ttu-id="00ccf-113">Importera runbooks eller konfigurationer från VSTS källkontroll när en incheckning är klar.</span><span class="sxs-lookup"><span data-stu-id="00ccf-113">Import runbooks or configurations from VSTS source control when a check-in is done.</span></span> <span data-ttu-id="00ccf-114">Om du kör manuellt den importera och publicera alla runbooks eller konfigurationer i Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="00ccf-114">If run manually, it will import and publish all runbooks or configurations into the Automation account.</span></span>| 
<span data-ttu-id="00ccf-115">Synkronisera VSTSGit</span><span class="sxs-lookup"><span data-stu-id="00ccf-115">Sync-VSTSGit</span></span> | <span data-ttu-id="00ccf-116">Importera runbooks eller konfigurationer från VSTS Git källkontroll när en incheckning är klar.</span><span class="sxs-lookup"><span data-stu-id="00ccf-116">Import runbooks or configurations from VSTS under Git source control when a check-in is done.</span></span> <span data-ttu-id="00ccf-117">Om du kör manuellt den importera och publicera alla runbooks eller konfigurationer i Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="00ccf-117">If run manually, it will import and publish all runbooks or configurations into the Automation account.</span></span>|

### <a name="variables"></a><span data-ttu-id="00ccf-118">Variabler</span><span class="sxs-lookup"><span data-stu-id="00ccf-118">Variables</span></span>

<span data-ttu-id="00ccf-119">Variabel</span><span class="sxs-lookup"><span data-stu-id="00ccf-119">Variable</span></span> | <span data-ttu-id="00ccf-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="00ccf-120">Description</span></span>|
-----------|------------|
<span data-ttu-id="00ccf-121">VSToken</span><span class="sxs-lookup"><span data-stu-id="00ccf-121">VSToken</span></span> | <span data-ttu-id="00ccf-122">Säker variabeltillgång skapar du som innehåller VSTS personliga åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="00ccf-122">Secure variable asset you will create that contains the VSTS personal access token.</span></span> <span data-ttu-id="00ccf-123">Du kan lära dig hur du skapar en personlig åtkomsttoken VSTS på den [VSTS autentiseringssidan](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview).</span><span class="sxs-lookup"><span data-stu-id="00ccf-123">You can learn how to create a VSTS personal access token on the [VSTS authentication page](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview).</span></span> 
## <a name="installing-and-configuring-this-scenario"></a><span data-ttu-id="00ccf-124">Installera och konfigurera det här scenariot</span><span class="sxs-lookup"><span data-stu-id="00ccf-124">Installing and configuring this scenario</span></span>

<span data-ttu-id="00ccf-125">Skapa en [personlig åtkomsttoken](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) i VSTS som du använder för att synkronisera runbooks eller konfigurationer i ditt automation-konto.</span><span class="sxs-lookup"><span data-stu-id="00ccf-125">Create a [personal access token](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) in VSTS that you will use to sync the runbooks or configurations into your automation account.</span></span>

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

<span data-ttu-id="00ccf-126">Skapa en [säker variabeln](automation-variables.md) i ditt automation-konto för att rymma personlig åtkomst-token så att runbook kan autentisera till VSTS och synkronisera runbooks eller konfigurationer i Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="00ccf-126">Create a [secure variable](automation-variables.md) in your automation account to hold the personal access token so that the runbook can authenticate to VSTS and sync the runbooks or configurations into the Automation account.</span></span> <span data-ttu-id="00ccf-127">Du kan kalla den här VSToken.</span><span class="sxs-lookup"><span data-stu-id="00ccf-127">You can name this VSToken.</span></span> 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

<span data-ttu-id="00ccf-128">Importera runbook som kommer att synkronisera dina runbooks eller konfigurationer till automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="00ccf-128">Import the runbook that will sync your runbooks or configurations into the automation account.</span></span> <span data-ttu-id="00ccf-129">Du kan använda den [VSTS exempel-runbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) eller [VSTS med Git exempel-runbook] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) från PowerShellGallery.com beroende på om du använder VSTS källa kontrollen eller VSTS med Git och distribuera dem till ditt automation-konto.</span><span class="sxs-lookup"><span data-stu-id="00ccf-129">You can use the [VSTS sample runbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) or the [VSTS with Git sample runbook] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) from the PowerShellGallery.com depending on if you use VSTS source control or VSTS with Git and deploy to your automation account.</span></span>

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

<span data-ttu-id="00ccf-130">Du kan nu [publicera](automation-creating-importing-runbook.md#publishing-a-runbook) denna runbook så att du kan skapa en webhook.</span><span class="sxs-lookup"><span data-stu-id="00ccf-130">You can now [publish](automation-creating-importing-runbook.md#publishing-a-runbook) this runbook so you can create a webhook.</span></span> 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

<span data-ttu-id="00ccf-131">Skapa en [webhook](automation-webhooks.md) för synkronisering VSTS runbook och Fyll i parametrarna som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="00ccf-131">Create a [webhook](automation-webhooks.md) for this Sync-VSTS runbook and fill in the parameters as shown below.</span></span> <span data-ttu-id="00ccf-132">Kontrollera att du kopierar webhooksadressen som du behöver för en tjänst hook i VSTS.</span><span class="sxs-lookup"><span data-stu-id="00ccf-132">Make sure you copy the webhook url as you will need it for a service hook in VSTS.</span></span> <span data-ttu-id="00ccf-133">VSAccessTokenVariableName är namn (VSToken) för säker variabeln som du skapade tidigare för att rymma den personliga åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="00ccf-133">The VSAccessTokenVariableName is the name (VSToken) of the secure variable that you created earlier to hold the personal access token.</span></span> 

<span data-ttu-id="00ccf-134">Integrera med VSTS (synkronisera VSTS.ps1) tar följande parametrar.</span><span class="sxs-lookup"><span data-stu-id="00ccf-134">Integrating with VSTS (Sync-VSTS.ps1) will take the following parameters.</span></span>
### <a name="sync-vsts-parameters"></a><span data-ttu-id="00ccf-135">Synkronisera VSTS parametrar</span><span class="sxs-lookup"><span data-stu-id="00ccf-135">Sync-VSTS Parameters</span></span>

<span data-ttu-id="00ccf-136">Parameter</span><span class="sxs-lookup"><span data-stu-id="00ccf-136">Parameter</span></span> | <span data-ttu-id="00ccf-137">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="00ccf-137">Description</span></span>| 
--------|------------|
<span data-ttu-id="00ccf-138">WebhookData</span><span class="sxs-lookup"><span data-stu-id="00ccf-138">WebhookData</span></span> | <span data-ttu-id="00ccf-139">Checka in informationen som skickas från VSTS service hook kommer att innehålla.</span><span class="sxs-lookup"><span data-stu-id="00ccf-139">This will contain the checkin information sent from the VSTS service hook.</span></span> <span data-ttu-id="00ccf-140">Den här parametern bör lämna tomt.</span><span class="sxs-lookup"><span data-stu-id="00ccf-140">You should leave this parameter blank.</span></span>| 
<span data-ttu-id="00ccf-141">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="00ccf-141">ResourceGroup</span></span> | <span data-ttu-id="00ccf-142">Detta är namnet på resursgruppen som automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="00ccf-142">This is the name of the resource group that the automation account is in.</span></span>|
<span data-ttu-id="00ccf-143">AutomationAccountName</span><span class="sxs-lookup"><span data-stu-id="00ccf-143">AutomationAccountName</span></span> | <span data-ttu-id="00ccf-144">Namnet på automation-kontot kommer att synkroniseras med VSTS.</span><span class="sxs-lookup"><span data-stu-id="00ccf-144">The name of the automation account that will sync with VSTS.</span></span>|
<span data-ttu-id="00ccf-145">VSFolder</span><span class="sxs-lookup"><span data-stu-id="00ccf-145">VSFolder</span></span> | <span data-ttu-id="00ccf-146">Namnet på mappen i VSTS där runbooks och konfigurationer finns.</span><span class="sxs-lookup"><span data-stu-id="00ccf-146">The name of the folder in VSTS where the runbooks and configurations exist.</span></span>|
<span data-ttu-id="00ccf-147">VSAccount</span><span class="sxs-lookup"><span data-stu-id="00ccf-147">VSAccount</span></span> | <span data-ttu-id="00ccf-148">Namnet på Visual Studio Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="00ccf-148">The name of the Visual Studio Team Services account.</span></span>| 
<span data-ttu-id="00ccf-149">VSAccessTokenVariableName</span><span class="sxs-lookup"><span data-stu-id="00ccf-149">VSAccessTokenVariableName</span></span> | <span data-ttu-id="00ccf-150">Namnet på den säkra variabel (VSToken) som innehåller VSTS personliga åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="00ccf-150">The name of the secure variable (VSToken) that holds the VSTS personal access token.</span></span>| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

<span data-ttu-id="00ccf-151">Om du använder VSTS med GIT (synkronisera VSTSGit.ps1) tar följande parametrar.</span><span class="sxs-lookup"><span data-stu-id="00ccf-151">If you are using VSTS with GIT (Sync-VSTSGit.ps1) it will take the following parameters.</span></span>

<span data-ttu-id="00ccf-152">Parameter</span><span class="sxs-lookup"><span data-stu-id="00ccf-152">Parameter</span></span> | <span data-ttu-id="00ccf-153">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="00ccf-153">Description</span></span>|
--------|------------|
<span data-ttu-id="00ccf-154">WebhookData</span><span class="sxs-lookup"><span data-stu-id="00ccf-154">WebhookData</span></span> | <span data-ttu-id="00ccf-155">Checka in informationen som skickas från VSTS service hook kommer att innehålla.</span><span class="sxs-lookup"><span data-stu-id="00ccf-155">This will contain the checkin information sent from the VSTS service hook.</span></span> <span data-ttu-id="00ccf-156">Den här parametern bör lämna tomt.</span><span class="sxs-lookup"><span data-stu-id="00ccf-156">You should leave this parameter blank.</span></span>| <span data-ttu-id="00ccf-157">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="00ccf-157">ResourceGroup</span></span> | <span data-ttu-id="00ccf-158">Det här namnet på resursgruppen som automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="00ccf-158">This the name of the resource group that the automation account is in.</span></span>|
<span data-ttu-id="00ccf-159">AutomationAccountName</span><span class="sxs-lookup"><span data-stu-id="00ccf-159">AutomationAccountName</span></span> | <span data-ttu-id="00ccf-160">Namnet på automation-kontot kommer att synkroniseras med VSTS.</span><span class="sxs-lookup"><span data-stu-id="00ccf-160">The name of the automation account that will sync with VSTS.</span></span>|
<span data-ttu-id="00ccf-161">VSAccount</span><span class="sxs-lookup"><span data-stu-id="00ccf-161">VSAccount</span></span> | <span data-ttu-id="00ccf-162">Namnet på Visual Studio Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="00ccf-162">The name of the Visual Studio Team Services account.</span></span>|
<span data-ttu-id="00ccf-163">VSProject</span><span class="sxs-lookup"><span data-stu-id="00ccf-163">VSProject</span></span> | <span data-ttu-id="00ccf-164">Namnet på projektet i VSTS där runbooks och konfigurationer finns.</span><span class="sxs-lookup"><span data-stu-id="00ccf-164">The name of the project in VSTS where the runbooks and configurations exist.</span></span>|
<span data-ttu-id="00ccf-165">GitRepo</span><span class="sxs-lookup"><span data-stu-id="00ccf-165">GitRepo</span></span> | <span data-ttu-id="00ccf-166">Namnet på Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="00ccf-166">The name of the Git repository.</span></span>|
<span data-ttu-id="00ccf-167">GitBranch</span><span class="sxs-lookup"><span data-stu-id="00ccf-167">GitBranch</span></span> | <span data-ttu-id="00ccf-168">Namnet på filialen i VSTS Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="00ccf-168">The name of the branch in VSTS Git repository.</span></span>|
<span data-ttu-id="00ccf-169">Mapp</span><span class="sxs-lookup"><span data-stu-id="00ccf-169">Folder</span></span> | <span data-ttu-id="00ccf-170">Namnet på mappen i VSTS Git grenen.</span><span class="sxs-lookup"><span data-stu-id="00ccf-170">The name of the folder in VSTS Git branch.</span></span>|
<span data-ttu-id="00ccf-171">VSAccessTokenVariableName</span><span class="sxs-lookup"><span data-stu-id="00ccf-171">VSAccessTokenVariableName</span></span> | <span data-ttu-id="00ccf-172">Namnet på den säkra variabel (VSToken) som innehåller VSTS personliga åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="00ccf-172">The name of the secure variable (VSToken) that holds the VSTS personal access token.</span></span>|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

<span data-ttu-id="00ccf-173">Skapa en tjänst hook i VSTS för incheckningar till den mapp som utlöser denna webhook på Checka in kod.</span><span class="sxs-lookup"><span data-stu-id="00ccf-173">Create a service hook in VSTS for check-ins to the folder that triggers this webhook on code check-in.</span></span> <span data-ttu-id="00ccf-174">Välj webbserver skapar som tjänsten för att integrera med när du skapar en ny prenumeration.</span><span class="sxs-lookup"><span data-stu-id="00ccf-174">Select Web Hooks as the service to integrate with when you create a new subscription.</span></span> <span data-ttu-id="00ccf-175">Du kan lära dig mer om tjänsten hook på [VSTS Service hook dokumentationen](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).</span><span class="sxs-lookup"><span data-stu-id="00ccf-175">You can learn more about service hooks on [VSTS Service Hooks documentation](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).</span></span>
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

<span data-ttu-id="00ccf-176">Du bör nu kunna göra alla incheckningar runbooks och konfigurationer i VSTS och låta dessa automatiskt synkronisering d i ditt automation-konto.</span><span class="sxs-lookup"><span data-stu-id="00ccf-176">You should now be able to do all check-ins of your runbooks and configurations into VSTS and have these automatically sync’d into your automation account.</span></span>

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

<span data-ttu-id="00ccf-177">Om du kör runbook manuellt utan som utlöses av VSTS parametern webhookdata du kan lämna tomt och den kommer att göra en fullständig synkronisering från mappen VSTS anges.</span><span class="sxs-lookup"><span data-stu-id="00ccf-177">If you run this runbook manually without being triggered by VSTS, you can leave the webhookdata parameter empty and it will do a full sync from the VSTS folder specified.</span></span>

<span data-ttu-id="00ccf-178">Om du vill avinstallera scenariot, ta bort tjänsten hook från VSTS bort runbook och variabeln VSToken.</span><span class="sxs-lookup"><span data-stu-id="00ccf-178">If you wish to uninstall the scenario, remove the service hook from VSTS, delete the runbook, and the VSToken variable.</span></span>
