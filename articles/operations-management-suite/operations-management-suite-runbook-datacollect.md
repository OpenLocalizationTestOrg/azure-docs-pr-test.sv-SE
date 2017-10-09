---
title: aaaCollecting Log Analytics-data med en runbook i Azure Automation | Microsoft Docs
description: "Stegvis självstudiekurs som går igenom hur du skapar en runbook i Azure Automation toocollect data i hello OMS-databas för analys av logganalys."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: operations-management-suite
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2017
ms.author: bwren
ms.openlocfilehash: e644dc3ef20fb1e930cae02e0fd44ccca31dc13d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a><span data-ttu-id="9fdf8-103">Samla in data i logganalys med en Azure Automation-runbook</span><span class="sxs-lookup"><span data-stu-id="9fdf8-103">Collect data in Log Analytics with an Azure Automation runbook</span></span>
<span data-ttu-id="9fdf8-104">Du kan samla in data i logganalys mycket från olika källor, till exempel [datakällor](../log-analytics/log-analytics-data-sources.md) på agenter och även [data som samlas in från Azure](../log-analytics/log-analytics-azure-storage.md).</span><span class="sxs-lookup"><span data-stu-id="9fdf8-104">You can collect a significant amount of data in Log Analytics from a variety of sources including [data sources](../log-analytics/log-analytics-data-sources.md) on agents and also [data collected from Azure](../log-analytics/log-analytics-azure-storage.md).</span></span>  <span data-ttu-id="9fdf8-105">Det finns en scenarier men där du behöver toocollect data som inte är tillgängligt via dessa källor som standard.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-105">There are a scenarios though where you need toocollect data that isn't accessible through these standard sources.</span></span>  <span data-ttu-id="9fdf8-106">I dessa fall kan du använda hello [HTTP Data Collector API: et](../log-analytics/log-analytics-data-collector-api.md) toowrite data tooLog Analytics från valfri REST API-klient.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-106">In these cases, you can use hello [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md) toowrite data tooLog Analytics from any REST API client.</span></span>  <span data-ttu-id="9fdf8-107">En gemensam metoden tooperform Datasamlingen använder en runbook i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-107">A common method tooperform this data collection is using a runbook in Azure Automation.</span></span>   

<span data-ttu-id="9fdf8-108">Den här kursen går igenom hello processen för att skapa och schemalägga en runbook i Azure Automation toowrite data tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-108">This tutorial walks through hello process for creating and scheduling a runbook in Azure Automation toowrite data tooLog Analytics.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="9fdf8-109">Krav</span><span class="sxs-lookup"><span data-stu-id="9fdf8-109">Prerequisites</span></span>
<span data-ttu-id="9fdf8-110">Det här scenariot kräver hello efter resurser som konfigurerats i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-110">This scenario requires hello following resources configured in your Azure subscription.</span></span>  <span data-ttu-id="9fdf8-111">Båda kan vara ett kostnadsfritt konto.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-111">Both can be a free account.</span></span>

- <span data-ttu-id="9fdf8-112">[Logga Analytics-arbetsyta](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9fdf8-112">[Log Analytics workspace](../log-analytics/log-analytics-get-started.md).</span></span>
- <span data-ttu-id="9fdf8-113">[Azure automation-konto](../automation/automation-offering-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9fdf8-113">[Azure automation account](../automation/automation-offering-get-started.md).</span></span>

## <a name="overview-of-scenario"></a><span data-ttu-id="9fdf8-114">Översikt över scenario</span><span class="sxs-lookup"><span data-stu-id="9fdf8-114">Overview of scenario</span></span>
<span data-ttu-id="9fdf8-115">Den här självstudiekursen skriver du en runbook som samlar in information om Automation-jobb.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-115">For this tutorial, you'll write a runbook that collects information about Automation jobs.</span></span>  <span data-ttu-id="9fdf8-116">Azure Automation-Runbooks implementeras med PowerShell, så att du ska börja med att skriva och testa ett skript i hello Azure Automation-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-116">Runbooks in Azure Automation are implemented with PowerShell, so you'll start by writing and testing a script in hello Azure Automation editor.</span></span>  <span data-ttu-id="9fdf8-117">När du har kontrollerat att du har samlat hello krävs information kan du skriva den data tooLog Analytics och verifiera hello anpassade datatypen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-117">Once you verify that you're collecting hello required information, you'll write that data tooLog Analytics and verify hello custom data type.</span></span>  <span data-ttu-id="9fdf8-118">Slutligen ska du skapa ett schema toostart hello runbook med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-118">Finally, you'll create a schedule toostart hello runbook at regular intervals.</span></span>

> [!NOTE]
> <span data-ttu-id="9fdf8-119">Du kan konfigurera Azure Automation toosend jobbet information tooLog Analytics utan denna runbook.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-119">You can configure Azure Automation toosend job information tooLog Analytics without this runbook.</span></span>  <span data-ttu-id="9fdf8-120">Det här scenariot är främst används toosupport hello självstudiekursen och det rekommenderas att skicka hello tooa test arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-120">This scenario is primarily used toosupport hello tutorial, and it's recommended that you send hello data tooa test workspace.</span></span>  


## <a name="1-install-data-collector-api-module"></a><span data-ttu-id="9fdf8-121">1. Installera Data Collector API-modulen</span><span class="sxs-lookup"><span data-stu-id="9fdf8-121">1. Install Data Collector API module</span></span>
<span data-ttu-id="9fdf8-122">Varje [begäran från hello HTTP Data Collector API: et](../log-analytics/log-analytics-data-collector-api.md#create-a-request) måste formateras på rätt sätt och innehålla ett authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-122">Every [request from hello HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md#create-a-request) must be formatted appropriately and include an authorization header.</span></span>  <span data-ttu-id="9fdf8-123">Du kan göra detta i din runbook, men du kan minska hello kod krävs med hjälp av en modul som förenklar processen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-123">You can do this in your runbook, but you can reduce hello amount of code required by using a module that simplifies this process.</span></span>  <span data-ttu-id="9fdf8-124">En modul som du kan använda är [OMSIngestionAPI modulen](https://www.powershellgallery.com/packages/OMSIngestionAPI) i hello PowerShell-galleriet.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-124">One module that you can use is [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI) in hello PowerShell Gallery.</span></span>

<span data-ttu-id="9fdf8-125">toouse en [modulen](../automation/automation-integration-modules.md) i en runbook måste den installeras i ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-125">toouse a [module](../automation/automation-integration-modules.md) in a runbook, it must be installed in your Automation account.</span></span>  <span data-ttu-id="9fdf8-126">Varje runbook i samma konto kan sedan använda hello hello funktioner i hello modul.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-126">Any runbook in hello same account can then use hello functions in hello module.</span></span>  <span data-ttu-id="9fdf8-127">Du kan installera en ny modul genom att välja **tillgångar** > **moduler** > **lägga till en modul** i Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-127">You can install a new module by selecting **Assets** > **Modules** > **Add a module** in your Automation account.</span></span>  

<span data-ttu-id="9fdf8-128">hello PowerShell-galleriet men ger dig en snabb alternativet toodeploy en modul direkt tooyour automation-konto så att du kan använda alternativet för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-128">hello PowerShell Gallery though gives you a quick option toodeploy a module directly tooyour automation account so you can use that option for this tutorial.</span></span>  

![OMSIngestionAPI modul](media/operations-management-suite-runbook-datacollect/OMSIngestionAPI.png)

1. <span data-ttu-id="9fdf8-130">Gå för[PowerShell-galleriet](https://www.powershellgallery.com/).</span><span class="sxs-lookup"><span data-stu-id="9fdf8-130">Go too[PowerShell Gallery](https://www.powershellgallery.com/).</span></span>
2. <span data-ttu-id="9fdf8-131">Sök efter **OMSIngestionAPI**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-131">Search for **OMSIngestionAPI**.</span></span>
3. <span data-ttu-id="9fdf8-132">Klicka på hello **distribuera tooAzure Automation** knappen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-132">Click on hello **Deploy tooAzure Automation** button.</span></span>
4. <span data-ttu-id="9fdf8-133">Välj ditt automation-konto och klicka på **OK** tooinstall hello modulen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-133">Select your automation account and click **OK** tooinstall hello module.</span></span>


## <a name="2-create-automation-variables"></a><span data-ttu-id="9fdf8-134">2. Skapa variabler för Automation</span><span class="sxs-lookup"><span data-stu-id="9fdf8-134">2. Create Automation variables</span></span>
<span data-ttu-id="9fdf8-135">[Automationsvariabler](..\automation\automation-variables.md) innehålla värden som kan användas av alla runbooks i ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-135">[Automation variables](..\automation\automation-variables.md) hold values that can be used by all runbooks in your Automation account.</span></span>  <span data-ttu-id="9fdf8-136">De gör runbooks mer flexibel genom att låta dig toochange dessa värden utan att redigera hello faktiska runbook.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-136">They make runbooks more flexible by allowing you toochange these values without editing hello actual runbook.</span></span> <span data-ttu-id="9fdf8-137">Varje begäran från hello HTTP Data Collector API kräver hello-ID och nyckeln för hello OMS-arbetsytan och variabeln tillgångar är perfekt toostore informationen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-137">Every request from hello HTTP Data Collector API requires hello ID and key of hello OMS workspace, and variable assets are ideal toostore this information.</span></span>  

![Variabler](media/operations-management-suite-runbook-datacollect/variables.png)

1. <span data-ttu-id="9fdf8-139">Navigera tooyour Automation-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-139">In hello Azure portal, navigate tooyour Automation account.</span></span>
2. <span data-ttu-id="9fdf8-140">Välj **variabler** under **delade resurser**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-140">Select **Variables** under **Shared Resources**.</span></span>
2. <span data-ttu-id="9fdf8-141">Klicka på **lägga till en variabel** och skapa hello två variabler i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-141">Click **Add a variable** and create hello two variables in hello following table.</span></span>

| <span data-ttu-id="9fdf8-142">Egenskap</span><span class="sxs-lookup"><span data-stu-id="9fdf8-142">Property</span></span> | <span data-ttu-id="9fdf8-143">Arbetsytan ID-värde</span><span class="sxs-lookup"><span data-stu-id="9fdf8-143">Workspace ID Value</span></span> | <span data-ttu-id="9fdf8-144">Nyckelvärdet för arbetsytan</span><span class="sxs-lookup"><span data-stu-id="9fdf8-144">Workspace Key Value</span></span> |
|:--|:--|:--|
| <span data-ttu-id="9fdf8-145">Namn</span><span class="sxs-lookup"><span data-stu-id="9fdf8-145">Name</span></span> | <span data-ttu-id="9fdf8-146">WorkspaceId</span><span class="sxs-lookup"><span data-stu-id="9fdf8-146">WorkspaceId</span></span> | <span data-ttu-id="9fdf8-147">WorkspaceKey</span><span class="sxs-lookup"><span data-stu-id="9fdf8-147">WorkspaceKey</span></span> |
| <span data-ttu-id="9fdf8-148">Typ</span><span class="sxs-lookup"><span data-stu-id="9fdf8-148">Type</span></span> | <span data-ttu-id="9fdf8-149">Sträng</span><span class="sxs-lookup"><span data-stu-id="9fdf8-149">String</span></span> | <span data-ttu-id="9fdf8-150">Sträng</span><span class="sxs-lookup"><span data-stu-id="9fdf8-150">String</span></span> |
| <span data-ttu-id="9fdf8-151">Värde</span><span class="sxs-lookup"><span data-stu-id="9fdf8-151">Value</span></span> | <span data-ttu-id="9fdf8-152">Klistra in i hello arbetsyte-ID för logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-152">Paste in hello Workspace ID of your Log Analytics workspace.</span></span> | <span data-ttu-id="9fdf8-153">Klistra in med hello primära eller sekundärnyckeln i logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-153">Paste in with hello Primary or Secondary Key of your Log Analytics workspace.</span></span> |
| <span data-ttu-id="9fdf8-154">Krypterade</span><span class="sxs-lookup"><span data-stu-id="9fdf8-154">Encrypted</span></span> | <span data-ttu-id="9fdf8-155">Nej</span><span class="sxs-lookup"><span data-stu-id="9fdf8-155">No</span></span> | <span data-ttu-id="9fdf8-156">Ja</span><span class="sxs-lookup"><span data-stu-id="9fdf8-156">Yes</span></span> |



## <a name="3-create-runbook"></a><span data-ttu-id="9fdf8-157">3. Skapa runbook</span><span class="sxs-lookup"><span data-stu-id="9fdf8-157">3. Create runbook</span></span>

<span data-ttu-id="9fdf8-158">Azure Automation har en redigerare i hello portal där du kan redigera och testa din runbook.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-158">Azure Automation has an editor in hello portal where you can edit and test your runbook.</span></span>  <span data-ttu-id="9fdf8-159">Du har hello alternativet toouse hello skript editor toowork med [PowerShell direkt](../automation/automation-edit-textual-runbook.md) eller [skapar en grafisk runbook](../automation/automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="9fdf8-159">You have hello option toouse hello script editor toowork with [PowerShell directly](../automation/automation-edit-textual-runbook.md) or [create a graphical runbook](../automation/automation-graphical-authoring-intro.md).</span></span>  <span data-ttu-id="9fdf8-160">För den här självstudiekursen kommer du arbeta med ett PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-160">For this tutorial, you will work with a PowerShell script.</span></span> 

![Redigera runbooken](media/operations-management-suite-runbook-datacollect/edit-runbook.png)

1. <span data-ttu-id="9fdf8-162">Navigera tooyour Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-162">Navigate tooyour Automation account.</span></span>  
2. <span data-ttu-id="9fdf8-163">Klicka på **Runbooks** > **lägga till en runbook** > **skapa en ny runbook**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-163">Click **Runbooks** > **Add a runbook** > **Create a new runbook**.</span></span>
3. <span data-ttu-id="9fdf8-164">Hej runbook-namn, Skriv **samla in-Automation-jobb**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-164">For hello runbook name, type **Collect-Automation-jobs**.</span></span>  <span data-ttu-id="9fdf8-165">Hej runbooktyp Välj **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-165">For hello runbook type, select **PowerShell**.</span></span>
4. <span data-ttu-id="9fdf8-166">Klicka på **skapa** toocreate hello runbook och starta hello redigeraren.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-166">Click **Create** toocreate hello runbook and start hello editor.</span></span>
5. <span data-ttu-id="9fdf8-167">Kopiera och klistra in följande kod i hello runbook hello.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-167">Copy and paste hello following code into hello runbook.</span></span>  <span data-ttu-id="9fdf8-168">Läs toohello kommentarer i hello skript förklaring av hello kod.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-168">Refer toohello comments in hello script for explanation of hello code.</span></span>
    
        # Get information required for hello automation account from parameter values when hello runbook is started.
        Param
        (
            [Parameter(Mandatory = $True)]
            [string]$resourceGroupName,
            [Parameter(Mandatory = $True)]
            [string]$automationAccountName
        )
        
        # Authenticate toohello Automation account using hello Azure connection created when hello Automation account was created.
        # Code copied from hello runbook AzureAutomationTutorial.
        $connectionName = "AzureRunAsConnection"
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
        Add-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
        
        # Set hello $VerbosePreference variable so that we get verbose output in test environment.
        $VerbosePreference = "Continue"
        
        # Get information required for Log Analytics workspace from Automation variables.
        $customerId = Get-AutomationVariable -Name 'WorkspaceID'
        $sharedKey = Get-AutomationVariable -Name 'WorkspaceKey'
        
        # Set hello name of hello record type.
        $logType = "AutomationJob"
        
        # Get hello jobs from hello past hour.
        $jobs = Get-AzureRmAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
        
        if ($jobs -ne $null) {
            # Convert hello job data toojson
            $body = $jobs | ConvertTo-Json
        
            # Write hello body tooverbose output so we can inspect it if verbose logging is on for hello runbook.
            Write-Verbose $body
        
            # Send hello data tooLog Analytics.
            Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
        }


## <a name="4-test-runbook"></a><span data-ttu-id="9fdf8-169">4. Testa runbook</span><span class="sxs-lookup"><span data-stu-id="9fdf8-169">4. Test runbook</span></span>
<span data-ttu-id="9fdf8-170">Azure Automation innehåller en miljö för[testa din runbook](../automation/automation-testing-runbook.md) innan du publicerar den.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-170">Azure Automation includes an environment too[test your runbook](../automation/automation-testing-runbook.md) before you publish it.</span></span>  <span data-ttu-id="9fdf8-171">Du kan inspektera hello data som samlas in av hello runbook och verifiera att skrivs tooLog Analytics som förväntat innan du publicerar den tooproduction.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-171">You can inspect hello data collected by hello runbook and verify that it writes tooLog Analytics as expected before publishing it tooproduction.</span></span> 
 
![Testa runbook](media/operations-management-suite-runbook-datacollect/test-runbook.png)

6. <span data-ttu-id="9fdf8-173">Klicka på **spara** toosave hello runbook.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-173">Click **Save** toosave hello runbook.</span></span>
1. <span data-ttu-id="9fdf8-174">Klicka på **Test fönstret** tooopen hello runbook i hello testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-174">Click **Test pane** tooopen hello runbook in hello test environment.</span></span>
3. <span data-ttu-id="9fdf8-175">Eftersom din runbook har parametrar, är du tillfrågas tooenter värden för dessa.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-175">Since your runbook has parameters, you're prompted tooenter values for them.</span></span>  <span data-ttu-id="9fdf8-176">Ange hello namnet på resursgruppen hello hello automation-kontot som kommer toocollect jobbinformation från.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-176">Enter hello name of hello resource group and hello automation account that your going toocollect job information from.</span></span>
4. <span data-ttu-id="9fdf8-177">Klicka på **starta** toohello starta hello runbook.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-177">Click **Start** toohello start hello runbook.</span></span>
3. <span data-ttu-id="9fdf8-178">Hej runbook startas med statusen **i kö** innan den försätts för**kör**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-178">hello runbook will start with a status of **Queued** before it goes too**Running**.</span></span>  
3. <span data-ttu-id="9fdf8-179">Hej runbook ska visa utförliga utdata med hello jobb som samlas in i json-format.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-179">hello runbook should display verbose output with hello jobs collected in json format.</span></span>  <span data-ttu-id="9fdf8-180">Om inga jobb visas sedan kanske har inga jobb skapas hello automation-konto i hello senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-180">If no jobs are listed, then there may have been no jobs created in hello automation account in hello last hour.</span></span>  <span data-ttu-id="9fdf8-181">Försök att starta en runbook i hello automation-kontot och utför sedan hello test igen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-181">Try starting any runbook in hello automation account and then perform hello test again.</span></span>
4. <span data-ttu-id="9fdf8-182">Se till att hello utdata inte visar eventuella fel i hello efter kommandot tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-182">Ensure that hello output doesn't show any errors in hello post command tooLog Analytics.</span></span>  <span data-ttu-id="9fdf8-183">Du bör ha ett meddelande liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-183">You should have a message similar toohello following.</span></span>

    ![Post-utdata](media/operations-management-suite-runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a><span data-ttu-id="9fdf8-185">5. Kontrollera posterna i logganalys</span><span class="sxs-lookup"><span data-stu-id="9fdf8-185">5. Verify records in Log Analytics</span></span>
<span data-ttu-id="9fdf8-186">När hello runbook har slutförts i test och du har kontrollerat att hello utdata har tagits emot kan du kontrollera att hello poster har skapats med en [loggen Sök i logganalys](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="9fdf8-186">Once hello runbook has completed in test, and you verified that hello output was successfully received, you can verify that hello records were created using a [log search in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

![Loggutdata saknas](media/operations-management-suite-runbook-datacollect/log-output.png)

1. <span data-ttu-id="9fdf8-188">Välj logganalys-arbetsytan i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-188">In hello Azure portal, select your Log Analytics workspace.</span></span>
2. <span data-ttu-id="9fdf8-189">Klicka på **logga Sök**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-189">Click on **Log Search**.</span></span>
3. <span data-ttu-id="9fdf8-190">Typen hello följande kommando `Type=AutomationJob_CL` och klicka på sökknappen hello.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-190">Type hello following command `Type=AutomationJob_CL` and click hello search button.</span></span> <span data-ttu-id="9fdf8-191">Observera att hello posttyp innehåller _CL som inte anges i hello skript.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-191">Note that hello record type includes _CL that isn't specified in hello script.</span></span>  <span data-ttu-id="9fdf8-192">Det suffixet är automatiskt tillagda toohello loggen typen tooindicate att det är en typ av anpassad logg.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-192">That suffix is automatically appended toohello log type tooindicate that it's a custom log type.</span></span>
4. <span data-ttu-id="9fdf8-193">Du bör se en eller flera poster returneras som anger att hello runbook fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-193">You should see one or more records returned indicating that hello runbook is working as expected.</span></span>


## <a name="6-publish-hello-runbook"></a><span data-ttu-id="9fdf8-194">6. Publicera hello runbook</span><span class="sxs-lookup"><span data-stu-id="9fdf8-194">6. Publish hello runbook</span></span>
<span data-ttu-id="9fdf8-195">När du har kontrollerat att hello runbooken fungerar korrekt, måste toopublish den så att du kan köra den i produktion.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-195">Once you've verified that hello runbook is working correctly, you need toopublish it so you can run it in production.</span></span>  <span data-ttu-id="9fdf8-196">Du kan fortsätta tooedit och testa hello runbook utan att ändra hello publicerad version.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-196">You can continue tooedit and test hello runbook without modifying hello published version.</span></span>  

![Publicera runbook](media/operations-management-suite-runbook-datacollect/publish-runbook.png)

1. <span data-ttu-id="9fdf8-198">Returnera tooyour automation-konto.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-198">Return tooyour automation account.</span></span>
2. <span data-ttu-id="9fdf8-199">Klicka på **Runbooks** och välj **samla in-Automation-jobb**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-199">Click on **Runbooks** and select **Collect-Automation-jobs**.</span></span>
3. <span data-ttu-id="9fdf8-200">Klicka på **redigera** och sedan **publicera**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-200">Click **Edit** and then **Publish**.</span></span>
4. <span data-ttu-id="9fdf8-201">Klicka på **Ja** när och tooverify som du vill toooverwrite hello tidigare publicerade versionen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-201">Click **Yes** when asked tooverify that you want toooverwrite hello previously published version.</span></span>

## <a name="7-set-logging-options"></a><span data-ttu-id="9fdf8-202">7. Ange alternativ för loggning</span><span class="sxs-lookup"><span data-stu-id="9fdf8-202">7. Set logging options</span></span> 
<span data-ttu-id="9fdf8-203">Testet du var kan tooview [utförlig utdata](../automation/automation-runbook-output-and-messages.md#message-streams) eftersom hello $VerbosePreference variabeln har angetts i hello skript.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-203">For test, you were able tooview [verbose output](../automation/automation-runbook-output-and-messages.md#message-streams) because you set hello $VerbosePreference variable in hello script.</span></span>  <span data-ttu-id="9fdf8-204">För produktion behöver du tooset hello Loggningsegenskaper för hello runbook om du vill tooview utförlig utdata.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-204">For production, you need tooset hello logging properties for hello runbook if you want tooview verbose output.</span></span>  <span data-ttu-id="9fdf8-205">För hello-runbook som används i den här självstudiekursen visas hello json-data skickas tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-205">For hello runbook used in this tutorial, this will display hello json data being sent tooLog Analytics.</span></span>

![Loggning och spårning](media/operations-management-suite-runbook-datacollect/logging.png)

1. <span data-ttu-id="9fdf8-207">Välj i hello egenskaperna för din runbook **loggning och spårning** under **Runbook-inställningar**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-207">In hello properties for your runbook select **Logging and tracing** under **Runbook Settings**.</span></span>
2. <span data-ttu-id="9fdf8-208">Ändra hello inställningen för **logga utförliga poster** för**på**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-208">Change hello setting for **Log verbose records** too**On**.</span></span>
3. <span data-ttu-id="9fdf8-209">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-209">Click **Save**.</span></span>

## <a name="8-schedule-runbook"></a><span data-ttu-id="9fdf8-210">8. Schemalägg runbook</span><span class="sxs-lookup"><span data-stu-id="9fdf8-210">8. Schedule runbook</span></span>
<span data-ttu-id="9fdf8-211">hello vanligaste sättet toostart en runbook som samlar in övervakningsdata är tooschedule den toorun automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-211">hello most common way toostart a runbook that collects monitoring data is tooschedule it toorun automatically.</span></span>  <span data-ttu-id="9fdf8-212">Det gör du genom att skapa en [schema i Azure Automation](../automation/automation-schedules.md) och kopplar den tooyour runbook.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-212">You do this by creating a [schedule in Azure Automation](../automation/automation-schedules.md) and attaching it tooyour runbook.</span></span>

![Schemalägg runbook](media/operations-management-suite-runbook-datacollect/schedule-runbook.png)

1. <span data-ttu-id="9fdf8-214">I hello egenskaper för din runbook, Välj **scheman** under **resurser**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-214">In hello properties for your runbook, select **Schedules** under **Resources**.</span></span>
2. <span data-ttu-id="9fdf8-215">Klicka på **lägga till ett schema** > **länka ett schema tooyour runbook** > **skapa ett nytt schema**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-215">Click **Add a schedule** > **Link a schedule tooyour runbook** > **Create a new schedule**.</span></span>
5. <span data-ttu-id="9fdf8-216">Typen i hello följande värden för hello schema och klickar på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-216">Type in hello following values for hello schedule and click **Create**.</span></span>

| <span data-ttu-id="9fdf8-217">Egenskap</span><span class="sxs-lookup"><span data-stu-id="9fdf8-217">Property</span></span> | <span data-ttu-id="9fdf8-218">Värde</span><span class="sxs-lookup"><span data-stu-id="9fdf8-218">Value</span></span> |
|:--|:--|
| <span data-ttu-id="9fdf8-219">Namn</span><span class="sxs-lookup"><span data-stu-id="9fdf8-219">Name</span></span> | <span data-ttu-id="9fdf8-220">AutomationJobs-varje timme</span><span class="sxs-lookup"><span data-stu-id="9fdf8-220">AutomationJobs-Hourly</span></span> |
| <span data-ttu-id="9fdf8-221">Startar</span><span class="sxs-lookup"><span data-stu-id="9fdf8-221">Starts</span></span> | <span data-ttu-id="9fdf8-222">Välj helst minst 5 minuter senaste hello aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-222">Select any time at least 5 minutes past hello current time.</span></span> |
| <span data-ttu-id="9fdf8-223">Upprepning</span><span class="sxs-lookup"><span data-stu-id="9fdf8-223">Recurrence</span></span> | <span data-ttu-id="9fdf8-224">Återkommande</span><span class="sxs-lookup"><span data-stu-id="9fdf8-224">Recurring</span></span> |
| <span data-ttu-id="9fdf8-225">Upprepas var</span><span class="sxs-lookup"><span data-stu-id="9fdf8-225">Recur every</span></span> | <span data-ttu-id="9fdf8-226">1 timme</span><span class="sxs-lookup"><span data-stu-id="9fdf8-226">1 hour</span></span> |
| <span data-ttu-id="9fdf8-227">Ange förfallodatum</span><span class="sxs-lookup"><span data-stu-id="9fdf8-227">Set expiration</span></span> | <span data-ttu-id="9fdf8-228">Nej</span><span class="sxs-lookup"><span data-stu-id="9fdf8-228">No</span></span> |

<span data-ttu-id="9fdf8-229">När hello schema har skapats måste tooset hello parametervärden som kommer att användas varje gång det här schemat startar hello runbook.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-229">Once hello schedule is created, you need tooset hello parameter values that will be used each time this schedule starts hello runbook.</span></span>

6. <span data-ttu-id="9fdf8-230">Klicka på **konfigurera parametrar och körningsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-230">Click **Configure parameters and run settings**.</span></span>
7. <span data-ttu-id="9fdf8-231">Fyll i värden för din **ResourceGroupName** och **AutomationAccountName**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-231">Fill in values for your **ResourceGroupName** and **AutomationAccountName**.</span></span>
8. <span data-ttu-id="9fdf8-232">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-232">Click **OK**.</span></span> 

## <a name="9-verify-runbook-starts-on-schedule"></a><span data-ttu-id="9fdf8-233">9. Kontrollera runbook startar enligt schema</span><span class="sxs-lookup"><span data-stu-id="9fdf8-233">9. Verify runbook starts on schedule</span></span>
<span data-ttu-id="9fdf8-234">Varje gång en runbook startas [ett jobb skapas](../automation/automation-runbook-execution.md) och all utdata som loggas.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-234">Everytime a runbook is started, [a job is created](../automation/automation-runbook-execution.md) and any output logged.</span></span>  <span data-ttu-id="9fdf8-235">Detta är i själva verket hello samlar in samma jobb som hello runbook.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-235">In fact, these are hello same jobs that hello runbook is collecting.</span></span>  <span data-ttu-id="9fdf8-236">Du kan kontrollera att hello runbook startar som förväntat genom att kontrollera hello jobb för runbook hello när hello starttiden för hello schema har överskridits.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-236">You can verify that hello runbook starts as expected by checking hello jobs for hello runbook after hello start time for hello schedule has passed.</span></span>

![Jobb](media/operations-management-suite-runbook-datacollect/jobs.png)

1. <span data-ttu-id="9fdf8-238">I hello egenskaper för din runbook, Välj **jobb** under **resurser**.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-238">In hello properties for your runbook, select **Jobs** under **Resources**.</span></span>
2. <span data-ttu-id="9fdf8-239">Du bör se en lista över jobb för varje gång hello runbook startades.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-239">You should see a listing of jobs for each time hello runbook was started.</span></span>
3. <span data-ttu-id="9fdf8-240">Klicka på någon av hello jobb tooview information.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-240">Click on one of hello jobs tooview its details.</span></span>
4. <span data-ttu-id="9fdf8-241">Klicka på **alla loggar** tooview hello loggar och utdata från hello runbook.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-241">Click on **All logs** tooview hello logs and output from hello runbook.</span></span>
5. <span data-ttu-id="9fdf8-242">Rulla toohello nedre toofind en post liknande toohello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-242">Scroll toohello bottom toofind an entry similar toohello image below.</span></span><br>![Utförlig](media/operations-management-suite-runbook-datacollect/verbose.png)
6. <span data-ttu-id="9fdf8-244">Klicka på den här posten tooview hello detaljerad json-data som skickades tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-244">Click on this entry tooview hello detailed json data  that was sent tooLog Analytics.</span></span>



## <a name="next-steps"></a><span data-ttu-id="9fdf8-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9fdf8-245">Next steps</span></span>
- <span data-ttu-id="9fdf8-246">Använd [Vydesigner](../log-analytics/log-analytics-view-designer.md) toocreate en vy som visar hello data som du har samlat in toohello logganalys-databasen.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-246">Use [View Designer](../log-analytics/log-analytics-view-designer.md) toocreate a view displaying hello data that you've collected toohello Log Analytics repository.</span></span>
- <span data-ttu-id="9fdf8-247">Paketera din runbook i en [hanteringslösning](operations-management-suite-solutions-creating.md) toodistribute toocustomers.</span><span class="sxs-lookup"><span data-stu-id="9fdf8-247">Package your runbook in a [management solution](operations-management-suite-solutions-creating.md) toodistribute toocustomers.</span></span>
- <span data-ttu-id="9fdf8-248">Lär dig mer om [logganalys](https://docs.microsoft.com/azure/log-analytics/).</span><span class="sxs-lookup"><span data-stu-id="9fdf8-248">Learn more about [Log Analytics](https://docs.microsoft.com/azure/log-analytics/).</span></span>
- <span data-ttu-id="9fdf8-249">Lär dig mer om [Azure Automation](https://docs.microsoft.com/azure/automation/).</span><span class="sxs-lookup"><span data-stu-id="9fdf8-249">Learn more about [Azure Automation](https://docs.microsoft.com/azure/automation/).</span></span>
- <span data-ttu-id="9fdf8-250">Mer information om hello [HTTP Data Collector API: et](../log-analytics/log-analytics-data-collector-api.md).</span><span class="sxs-lookup"><span data-stu-id="9fdf8-250">Learn more about hello [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md).</span></span>
