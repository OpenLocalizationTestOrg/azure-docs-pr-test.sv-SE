---
title: "Insamling av data från logganalys med en runbook i Azure Automation | Microsoft Docs"
description: "Stegvis självstudiekurs som går igenom hur du skapar en runbook i Azure Automation för att samla in data i OMS-databas för analys av logganalys."
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
ms.openlocfilehash: 59f674c9c6404da7f5384539189f41a4ba1a939a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a><span data-ttu-id="3e481-103">Samla in data i logganalys med en Azure Automation-runbook</span><span class="sxs-lookup"><span data-stu-id="3e481-103">Collect data in Log Analytics with an Azure Automation runbook</span></span>
<span data-ttu-id="3e481-104">Du kan samla in data i logganalys mycket från olika källor, till exempel [datakällor](../log-analytics/log-analytics-data-sources.md) på agenter och även [data som samlas in från Azure](../log-analytics/log-analytics-azure-storage.md).</span><span class="sxs-lookup"><span data-stu-id="3e481-104">You can collect a significant amount of data in Log Analytics from a variety of sources including [data sources](../log-analytics/log-analytics-data-sources.md) on agents and also [data collected from Azure](../log-analytics/log-analytics-azure-storage.md).</span></span>  <span data-ttu-id="3e481-105">Det finns en scenarier men där du behöver samla in data som inte är tillgängligt via dessa källor som standard.</span><span class="sxs-lookup"><span data-stu-id="3e481-105">There are a scenarios though where you need to collect data that isn't accessible through these standard sources.</span></span>  <span data-ttu-id="3e481-106">I dessa fall kan du använda den [HTTP Data Collector API: et](../log-analytics/log-analytics-data-collector-api.md) att skriva data till logganalys från valfri REST API-klient.</span><span class="sxs-lookup"><span data-stu-id="3e481-106">In these cases, you can use the [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md) to write data to Log Analytics from any REST API client.</span></span>  <span data-ttu-id="3e481-107">En vanlig metod för att utföra den här Datasamlingen använder en runbook i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="3e481-107">A common method to perform this data collection is using a runbook in Azure Automation.</span></span>   

<span data-ttu-id="3e481-108">Den här kursen går igenom processen för att skapa och schemalägga en runbook i Azure Automation för att skriva data till logganalys.</span><span class="sxs-lookup"><span data-stu-id="3e481-108">This tutorial walks through the process for creating and scheduling a runbook in Azure Automation to write data to Log Analytics.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="3e481-109">Krav</span><span class="sxs-lookup"><span data-stu-id="3e481-109">Prerequisites</span></span>
<span data-ttu-id="3e481-110">Det här scenariot kräver följande resurser som konfigurerats i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3e481-110">This scenario requires the following resources configured in your Azure subscription.</span></span>  <span data-ttu-id="3e481-111">Båda kan vara ett kostnadsfritt konto.</span><span class="sxs-lookup"><span data-stu-id="3e481-111">Both can be a free account.</span></span>

- <span data-ttu-id="3e481-112">[Logga Analytics-arbetsyta](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3e481-112">[Log Analytics workspace](../log-analytics/log-analytics-get-started.md).</span></span>
- <span data-ttu-id="3e481-113">[Azure automation-konto](../automation/automation-offering-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3e481-113">[Azure automation account](../automation/automation-offering-get-started.md).</span></span>

## <a name="overview-of-scenario"></a><span data-ttu-id="3e481-114">Översikt över scenario</span><span class="sxs-lookup"><span data-stu-id="3e481-114">Overview of scenario</span></span>
<span data-ttu-id="3e481-115">Den här självstudiekursen skriver du en runbook som samlar in information om Automation-jobb.</span><span class="sxs-lookup"><span data-stu-id="3e481-115">For this tutorial, you'll write a runbook that collects information about Automation jobs.</span></span>  <span data-ttu-id="3e481-116">Azure Automation-Runbooks implementeras med PowerShell, så att du ska börja med att skriva och testa ett skript i Azure Automation-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="3e481-116">Runbooks in Azure Automation are implemented with PowerShell, so you'll start by writing and testing a script in the Azure Automation editor.</span></span>  <span data-ttu-id="3e481-117">När du har kontrollerat att du har samlat informationen som krävs kan du skriva data till logganalys och verifiera den anpassade datatypen.</span><span class="sxs-lookup"><span data-stu-id="3e481-117">Once you verify that you're collecting the required information, you'll write that data to Log Analytics and verify the custom data type.</span></span>  <span data-ttu-id="3e481-118">Slutligen ska du skapa ett schema för att starta runbook med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="3e481-118">Finally, you'll create a schedule to start the runbook at regular intervals.</span></span>

> [!NOTE]
> <span data-ttu-id="3e481-119">Du kan konfigurera Azure Automation för att skicka information om jobbets till logganalys utan denna runbook.</span><span class="sxs-lookup"><span data-stu-id="3e481-119">You can configure Azure Automation to send job information to Log Analytics without this runbook.</span></span>  <span data-ttu-id="3e481-120">Det här scenariot används främst att stödja kursen och det rekommenderas att du skickar data till en test-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="3e481-120">This scenario is primarily used to support the tutorial, and it's recommended that you send the data to a test workspace.</span></span>  


## <a name="1-install-data-collector-api-module"></a><span data-ttu-id="3e481-121">1. Installera Data Collector API-modulen</span><span class="sxs-lookup"><span data-stu-id="3e481-121">1. Install Data Collector API module</span></span>
<span data-ttu-id="3e481-122">Varje [begäran från API: et för HTTP-Data Collector](../log-analytics/log-analytics-data-collector-api.md#create-a-request) måste formateras på rätt sätt och innehålla ett authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="3e481-122">Every [request from the HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md#create-a-request) must be formatted appropriately and include an authorization header.</span></span>  <span data-ttu-id="3e481-123">Du kan göra detta i din runbook, men du kan minska den kod som krävs med hjälp av en modul som förenklar processen.</span><span class="sxs-lookup"><span data-stu-id="3e481-123">You can do this in your runbook, but you can reduce the amount of code required by using a module that simplifies this process.</span></span>  <span data-ttu-id="3e481-124">En modul som du kan använda är [OMSIngestionAPI modulen](https://www.powershellgallery.com/packages/OMSIngestionAPI) i PowerShell-galleriet.</span><span class="sxs-lookup"><span data-stu-id="3e481-124">One module that you can use is [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI) in the PowerShell Gallery.</span></span>

<span data-ttu-id="3e481-125">Att använda en [modulen](../automation/automation-integration-modules.md) i en runbook måste den installeras i ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="3e481-125">To use a [module](../automation/automation-integration-modules.md) in a runbook, it must be installed in your Automation account.</span></span>  <span data-ttu-id="3e481-126">Varje runbook i samma konto kan sedan använda funktionerna i modulen.</span><span class="sxs-lookup"><span data-stu-id="3e481-126">Any runbook in the same account can then use the functions in the module.</span></span>  <span data-ttu-id="3e481-127">Du kan installera en ny modul genom att välja **tillgångar** > **moduler** > **lägga till en modul** i Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="3e481-127">You can install a new module by selecting **Assets** > **Modules** > **Add a module** in your Automation account.</span></span>  

<span data-ttu-id="3e481-128">PowerShell-galleriet ger dig även om ett snabbt alternativ att distribuera en modul direkt till ditt automation-konto så att du kan använda alternativet för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="3e481-128">The PowerShell Gallery though gives you a quick option to deploy a module directly to your automation account so you can use that option for this tutorial.</span></span>  

![OMSIngestionAPI modul](media/operations-management-suite-runbook-datacollect/OMSIngestionAPI.png)

1. <span data-ttu-id="3e481-130">Gå till [PowerShell-galleriet](https://www.powershellgallery.com/).</span><span class="sxs-lookup"><span data-stu-id="3e481-130">Go to [PowerShell Gallery](https://www.powershellgallery.com/).</span></span>
2. <span data-ttu-id="3e481-131">Sök efter **OMSIngestionAPI**.</span><span class="sxs-lookup"><span data-stu-id="3e481-131">Search for **OMSIngestionAPI**.</span></span>
3. <span data-ttu-id="3e481-132">Klicka på den **till Azure Automation** knappen.</span><span class="sxs-lookup"><span data-stu-id="3e481-132">Click on the **Deploy to Azure Automation** button.</span></span>
4. <span data-ttu-id="3e481-133">Välj ditt automation-konto och klicka på **OK** att installera modulen.</span><span class="sxs-lookup"><span data-stu-id="3e481-133">Select your automation account and click **OK** to install the module.</span></span>


## <a name="2-create-automation-variables"></a><span data-ttu-id="3e481-134">2. Skapa variabler för Automation</span><span class="sxs-lookup"><span data-stu-id="3e481-134">2. Create Automation variables</span></span>
<span data-ttu-id="3e481-135">[Automationsvariabler](..\automation\automation-variables.md) innehålla värden som kan användas av alla runbooks i ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="3e481-135">[Automation variables](..\automation\automation-variables.md) hold values that can be used by all runbooks in your Automation account.</span></span>  <span data-ttu-id="3e481-136">De göra runbooks mer flexibel genom att du kan ändra dessa värden utan att redigera den faktiska runbooken.</span><span class="sxs-lookup"><span data-stu-id="3e481-136">They make runbooks more flexible by allowing you to change these values without editing the actual runbook.</span></span> <span data-ttu-id="3e481-137">Varje begäran från http-Data Collector API kräver ID och nyckel i OMS-arbetsytan och variabeln tillgångar är perfekt att lagra informationen.</span><span class="sxs-lookup"><span data-stu-id="3e481-137">Every request from the HTTP Data Collector API requires the ID and key of the OMS workspace, and variable assets are ideal to store this information.</span></span>  

![Variabler](media/operations-management-suite-runbook-datacollect/variables.png)

1. <span data-ttu-id="3e481-139">Navigera till ditt Automation-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3e481-139">In the Azure portal, navigate to your Automation account.</span></span>
2. <span data-ttu-id="3e481-140">Välj **variabler** under **delade resurser**.</span><span class="sxs-lookup"><span data-stu-id="3e481-140">Select **Variables** under **Shared Resources**.</span></span>
2. <span data-ttu-id="3e481-141">Klicka på **lägga till en variabel** och skapa två variabler i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="3e481-141">Click **Add a variable** and create the two variables in the following table.</span></span>

| <span data-ttu-id="3e481-142">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3e481-142">Property</span></span> | <span data-ttu-id="3e481-143">Arbetsytan ID-värde</span><span class="sxs-lookup"><span data-stu-id="3e481-143">Workspace ID Value</span></span> | <span data-ttu-id="3e481-144">Nyckelvärdet för arbetsytan</span><span class="sxs-lookup"><span data-stu-id="3e481-144">Workspace Key Value</span></span> |
|:--|:--|:--|
| <span data-ttu-id="3e481-145">Namn</span><span class="sxs-lookup"><span data-stu-id="3e481-145">Name</span></span> | <span data-ttu-id="3e481-146">WorkspaceId</span><span class="sxs-lookup"><span data-stu-id="3e481-146">WorkspaceId</span></span> | <span data-ttu-id="3e481-147">WorkspaceKey</span><span class="sxs-lookup"><span data-stu-id="3e481-147">WorkspaceKey</span></span> |
| <span data-ttu-id="3e481-148">Typ</span><span class="sxs-lookup"><span data-stu-id="3e481-148">Type</span></span> | <span data-ttu-id="3e481-149">Sträng</span><span class="sxs-lookup"><span data-stu-id="3e481-149">String</span></span> | <span data-ttu-id="3e481-150">Sträng</span><span class="sxs-lookup"><span data-stu-id="3e481-150">String</span></span> |
| <span data-ttu-id="3e481-151">Värde</span><span class="sxs-lookup"><span data-stu-id="3e481-151">Value</span></span> | <span data-ttu-id="3e481-152">Klistra in i ditt arbetsyte-ID för logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="3e481-152">Paste in the Workspace ID of your Log Analytics workspace.</span></span> | <span data-ttu-id="3e481-153">Klistra in med primärt eller sekundärnyckeln i logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="3e481-153">Paste in with the Primary or Secondary Key of your Log Analytics workspace.</span></span> |
| <span data-ttu-id="3e481-154">Krypterade</span><span class="sxs-lookup"><span data-stu-id="3e481-154">Encrypted</span></span> | <span data-ttu-id="3e481-155">Nej</span><span class="sxs-lookup"><span data-stu-id="3e481-155">No</span></span> | <span data-ttu-id="3e481-156">Ja</span><span class="sxs-lookup"><span data-stu-id="3e481-156">Yes</span></span> |



## <a name="3-create-runbook"></a><span data-ttu-id="3e481-157">3. Skapa runbook</span><span class="sxs-lookup"><span data-stu-id="3e481-157">3. Create runbook</span></span>

<span data-ttu-id="3e481-158">Azure Automation har en redigerare i portalen där du kan redigera och testa din runbook.</span><span class="sxs-lookup"><span data-stu-id="3e481-158">Azure Automation has an editor in the portal where you can edit and test your runbook.</span></span>  <span data-ttu-id="3e481-159">Du har möjlighet att använda Skriptredigeraren för att arbeta med [PowerShell direkt](../automation/automation-edit-textual-runbook.md) eller [skapar en grafisk runbook](../automation/automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="3e481-159">You have the option to use the script editor to work with [PowerShell directly](../automation/automation-edit-textual-runbook.md) or [create a graphical runbook](../automation/automation-graphical-authoring-intro.md).</span></span>  <span data-ttu-id="3e481-160">För den här självstudiekursen kommer du arbeta med ett PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="3e481-160">For this tutorial, you will work with a PowerShell script.</span></span> 

![Redigera runbooken](media/operations-management-suite-runbook-datacollect/edit-runbook.png)

1. <span data-ttu-id="3e481-162">Navigera till ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="3e481-162">Navigate to your Automation account.</span></span>  
2. <span data-ttu-id="3e481-163">Klicka på **Runbooks** > **lägga till en runbook** > **skapa en ny runbook**.</span><span class="sxs-lookup"><span data-stu-id="3e481-163">Click **Runbooks** > **Add a runbook** > **Create a new runbook**.</span></span>
3. <span data-ttu-id="3e481-164">Runbook-namnet skriver **samla in-Automation-jobb**.</span><span class="sxs-lookup"><span data-stu-id="3e481-164">For the runbook name, type **Collect-Automation-jobs**.</span></span>  <span data-ttu-id="3e481-165">Vilken runbooktyp av, Välj **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="3e481-165">For the runbook type, select **PowerShell**.</span></span>
4. <span data-ttu-id="3e481-166">Klicka på **skapa** skapa runbook och starta redigeraren.</span><span class="sxs-lookup"><span data-stu-id="3e481-166">Click **Create** to create the runbook and start the editor.</span></span>
5. <span data-ttu-id="3e481-167">Kopiera och klistra in följande kod i runbook.</span><span class="sxs-lookup"><span data-stu-id="3e481-167">Copy and paste the following code into the runbook.</span></span>  <span data-ttu-id="3e481-168">Referera till kommentarerna i skriptet förklaring av koden.</span><span class="sxs-lookup"><span data-stu-id="3e481-168">Refer to the comments in the script for explanation of the code.</span></span>
    
        # Get information required for the automation account from parameter values when the runbook is started.
        Param
        (
            [Parameter(Mandatory = $True)]
            [string]$resourceGroupName,
            [Parameter(Mandatory = $True)]
            [string]$automationAccountName
        )
        
        # Authenticate to the Automation account using the Azure connection created when the Automation account was created.
        # Code copied from the runbook AzureAutomationTutorial.
        $connectionName = "AzureRunAsConnection"
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
        Add-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
        
        # Set the $VerbosePreference variable so that we get verbose output in test environment.
        $VerbosePreference = "Continue"
        
        # Get information required for Log Analytics workspace from Automation variables.
        $customerId = Get-AutomationVariable -Name 'WorkspaceID'
        $sharedKey = Get-AutomationVariable -Name 'WorkspaceKey'
        
        # Set the name of the record type.
        $logType = "AutomationJob"
        
        # Get the jobs from the past hour.
        $jobs = Get-AzureRmAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
        
        if ($jobs -ne $null) {
            # Convert the job data to json
            $body = $jobs | ConvertTo-Json
        
            # Write the body to verbose output so we can inspect it if verbose logging is on for the runbook.
            Write-Verbose $body
        
            # Send the data to Log Analytics.
            Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
        }


## <a name="4-test-runbook"></a><span data-ttu-id="3e481-169">4. Testa runbook</span><span class="sxs-lookup"><span data-stu-id="3e481-169">4. Test runbook</span></span>
<span data-ttu-id="3e481-170">Azure Automation innehåller en miljö till [testa din runbook](../automation/automation-testing-runbook.md) innan du publicerar den.</span><span class="sxs-lookup"><span data-stu-id="3e481-170">Azure Automation includes an environment to [test your runbook](../automation/automation-testing-runbook.md) before you publish it.</span></span>  <span data-ttu-id="3e481-171">Du kan granska de data som samlas in av runbook och verifiera att den skrivs till logganalys som förväntat innan du publicerar den till produktion.</span><span class="sxs-lookup"><span data-stu-id="3e481-171">You can inspect the data collected by the runbook and verify that it writes to Log Analytics as expected before publishing it to production.</span></span> 
 
![Testa runbook](media/operations-management-suite-runbook-datacollect/test-runbook.png)

6. <span data-ttu-id="3e481-173">Klicka på **spara** att spara runbook.</span><span class="sxs-lookup"><span data-stu-id="3e481-173">Click **Save** to save the runbook.</span></span>
1. <span data-ttu-id="3e481-174">Klicka på **Test fönstret** öppna runbook i testmiljön.</span><span class="sxs-lookup"><span data-stu-id="3e481-174">Click **Test pane** to open the runbook in the test environment.</span></span>
3. <span data-ttu-id="3e481-175">Eftersom din runbook har parametrar uppmanas du att ange värden för dessa.</span><span class="sxs-lookup"><span data-stu-id="3e481-175">Since your runbook has parameters, you're prompted to enter values for them.</span></span>  <span data-ttu-id="3e481-176">Ange namnet på resursgruppen och automatisering kontot som kommer att samla in information om jobbets från.</span><span class="sxs-lookup"><span data-stu-id="3e481-176">Enter the name of the resource group and the automation account that your going to collect job information from.</span></span>
4. <span data-ttu-id="3e481-177">Klicka på **starta** att starta runbook.</span><span class="sxs-lookup"><span data-stu-id="3e481-177">Click **Start** to the start the runbook.</span></span>
3. <span data-ttu-id="3e481-178">Runbook startas med statusen **i kö** innan den går att **kör**.</span><span class="sxs-lookup"><span data-stu-id="3e481-178">The runbook will start with a status of **Queued** before it goes to **Running**.</span></span>  
3. <span data-ttu-id="3e481-179">Runbook ska visa utförliga utdata med de jobb som samlas in i json-format.</span><span class="sxs-lookup"><span data-stu-id="3e481-179">The runbook should display verbose output with the jobs collected in json format.</span></span>  <span data-ttu-id="3e481-180">Om inga jobb visas sedan kanske har inga jobb som skapats i automation-konto i den senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="3e481-180">If no jobs are listed, then there may have been no jobs created in the automation account in the last hour.</span></span>  <span data-ttu-id="3e481-181">Försök att starta en runbook i automation-konto och utföra testet igen.</span><span class="sxs-lookup"><span data-stu-id="3e481-181">Try starting any runbook in the automation account and then perform the test again.</span></span>
4. <span data-ttu-id="3e481-182">Kontrollera att resultatet inte visas några fel i kommandot post till logganalys.</span><span class="sxs-lookup"><span data-stu-id="3e481-182">Ensure that the output doesn't show any errors in the post command to Log Analytics.</span></span>  <span data-ttu-id="3e481-183">Du bör ha ett meddelande som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="3e481-183">You should have a message similar to the following.</span></span>

    ![Post-utdata](media/operations-management-suite-runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a><span data-ttu-id="3e481-185">5. Kontrollera posterna i logganalys</span><span class="sxs-lookup"><span data-stu-id="3e481-185">5. Verify records in Log Analytics</span></span>
<span data-ttu-id="3e481-186">När runbooken har slutförts i test och du har kontrollerat att utdata har tagits emot kan du verifiera att posterna har skapats med en [loggen Sök i logganalys](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="3e481-186">Once the runbook has completed in test, and you verified that the output was successfully received, you can verify that the records were created using a [log search in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

![Loggutdata saknas](media/operations-management-suite-runbook-datacollect/log-output.png)

1. <span data-ttu-id="3e481-188">Välj logganalys-arbetsytan i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3e481-188">In the Azure portal, select your Log Analytics workspace.</span></span>
2. <span data-ttu-id="3e481-189">Klicka på **logga Sök**.</span><span class="sxs-lookup"><span data-stu-id="3e481-189">Click on **Log Search**.</span></span>
3. <span data-ttu-id="3e481-190">Skriv följande kommando `Type=AutomationJob_CL` och klicka på sökknappen.</span><span class="sxs-lookup"><span data-stu-id="3e481-190">Type the following command `Type=AutomationJob_CL` and click the search button.</span></span> <span data-ttu-id="3e481-191">Observera att posttypen som innehåller _CL som inte anges i skriptet.</span><span class="sxs-lookup"><span data-stu-id="3e481-191">Note that the record type includes _CL that isn't specified in the script.</span></span>  <span data-ttu-id="3e481-192">Det suffixet läggs automatiskt till loggtyp som indikerar att en anpassad logg-typen.</span><span class="sxs-lookup"><span data-stu-id="3e481-192">That suffix is automatically appended to the log type to indicate that it's a custom log type.</span></span>
4. <span data-ttu-id="3e481-193">Du bör se en eller flera poster returneras som anger att runbooken fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="3e481-193">You should see one or more records returned indicating that the runbook is working as expected.</span></span>


## <a name="6-publish-the-runbook"></a><span data-ttu-id="3e481-194">6. Publicera en runbook</span><span class="sxs-lookup"><span data-stu-id="3e481-194">6. Publish the runbook</span></span>
<span data-ttu-id="3e481-195">När du har kontrollerat att runbooken fungerar korrekt, måste du publicera den så att du kan köra den i produktion.</span><span class="sxs-lookup"><span data-stu-id="3e481-195">Once you've verified that the runbook is working correctly, you need to publish it so you can run it in production.</span></span>  <span data-ttu-id="3e481-196">Du kan fortsätta att redigera och testa runbook utan att ändra den publicerade versionen.</span><span class="sxs-lookup"><span data-stu-id="3e481-196">You can continue to edit and test the runbook without modifying the published version.</span></span>  

![Publicera runbook](media/operations-management-suite-runbook-datacollect/publish-runbook.png)

1. <span data-ttu-id="3e481-198">Gå tillbaka till ditt automation-konto.</span><span class="sxs-lookup"><span data-stu-id="3e481-198">Return to your automation account.</span></span>
2. <span data-ttu-id="3e481-199">Klicka på **Runbooks** och välj **samla in-Automation-jobb**.</span><span class="sxs-lookup"><span data-stu-id="3e481-199">Click on **Runbooks** and select **Collect-Automation-jobs**.</span></span>
3. <span data-ttu-id="3e481-200">Klicka på **redigera** och sedan **publicera**.</span><span class="sxs-lookup"><span data-stu-id="3e481-200">Click **Edit** and then **Publish**.</span></span>
4. <span data-ttu-id="3e481-201">Klicka på **Ja** när du ombeds bekräfta att du vill skriva över den tidigare publicerade versionen.</span><span class="sxs-lookup"><span data-stu-id="3e481-201">Click **Yes** when asked to verify that you want to overwrite the previously published version.</span></span>

## <a name="7-set-logging-options"></a><span data-ttu-id="3e481-202">7. Ange alternativ för loggning</span><span class="sxs-lookup"><span data-stu-id="3e481-202">7. Set logging options</span></span> 
<span data-ttu-id="3e481-203">Test, var du kan visa [utförlig utdata](../automation/automation-runbook-output-and-messages.md#message-streams) eftersom du anger variabeln $VerbosePreference i skriptet.</span><span class="sxs-lookup"><span data-stu-id="3e481-203">For test, you were able to view [verbose output](../automation/automation-runbook-output-and-messages.md#message-streams) because you set the $VerbosePreference variable in the script.</span></span>  <span data-ttu-id="3e481-204">Du måste ange loggningsegenskaperna för runbook om du vill visa detaljerade utdata för produktion.</span><span class="sxs-lookup"><span data-stu-id="3e481-204">For production, you need to set the logging properties for the runbook if you want to view verbose output.</span></span>  <span data-ttu-id="3e481-205">För den runbook som används i den här självstudiekursen visas json-data som skickas till logganalys.</span><span class="sxs-lookup"><span data-stu-id="3e481-205">For the runbook used in this tutorial, this will display the json data being sent to Log Analytics.</span></span>

![Loggning och spårning](media/operations-management-suite-runbook-datacollect/logging.png)

1. <span data-ttu-id="3e481-207">I egenskaperna för din runbook **loggning och spårning** under **Runbook-inställningar**.</span><span class="sxs-lookup"><span data-stu-id="3e481-207">In the properties for your runbook select **Logging and tracing** under **Runbook Settings**.</span></span>
2. <span data-ttu-id="3e481-208">Ändra inställningen för **logga utförliga poster** till **på**.</span><span class="sxs-lookup"><span data-stu-id="3e481-208">Change the setting for **Log verbose records** to **On**.</span></span>
3. <span data-ttu-id="3e481-209">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3e481-209">Click **Save**.</span></span>

## <a name="8-schedule-runbook"></a><span data-ttu-id="3e481-210">8. Schemalägg runbook</span><span class="sxs-lookup"><span data-stu-id="3e481-210">8. Schedule runbook</span></span>
<span data-ttu-id="3e481-211">Det vanligaste sättet att starta en runbook som samlar in övervakningsdata är att schemalägga startas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3e481-211">The most common way to start a runbook that collects monitoring data is to schedule it to run automatically.</span></span>  <span data-ttu-id="3e481-212">Det gör du genom att skapa en [schema i Azure Automation](../automation/automation-schedules.md) och kopplar den till din runbook.</span><span class="sxs-lookup"><span data-stu-id="3e481-212">You do this by creating a [schedule in Azure Automation](../automation/automation-schedules.md) and attaching it to your runbook.</span></span>

![Schemalägg runbook](media/operations-management-suite-runbook-datacollect/schedule-runbook.png)

1. <span data-ttu-id="3e481-214">I egenskaperna för din runbook, Välj **scheman** under **resurser**.</span><span class="sxs-lookup"><span data-stu-id="3e481-214">In the properties for your runbook, select **Schedules** under **Resources**.</span></span>
2. <span data-ttu-id="3e481-215">Klicka på **lägga till ett schema** > **länka ett schema till din runbook** > **skapa ett nytt schema**.</span><span class="sxs-lookup"><span data-stu-id="3e481-215">Click **Add a schedule** > **Link a schedule to your runbook** > **Create a new schedule**.</span></span>
5. <span data-ttu-id="3e481-216">Ange följande värden för schemat och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3e481-216">Type in the following values for the schedule and click **Create**.</span></span>

| <span data-ttu-id="3e481-217">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3e481-217">Property</span></span> | <span data-ttu-id="3e481-218">Värde</span><span class="sxs-lookup"><span data-stu-id="3e481-218">Value</span></span> |
|:--|:--|
| <span data-ttu-id="3e481-219">Namn</span><span class="sxs-lookup"><span data-stu-id="3e481-219">Name</span></span> | <span data-ttu-id="3e481-220">AutomationJobs-varje timme</span><span class="sxs-lookup"><span data-stu-id="3e481-220">AutomationJobs-Hourly</span></span> |
| <span data-ttu-id="3e481-221">Startar</span><span class="sxs-lookup"><span data-stu-id="3e481-221">Starts</span></span> | <span data-ttu-id="3e481-222">Välj när minst 5 minuter efter den aktuella tiden.</span><span class="sxs-lookup"><span data-stu-id="3e481-222">Select any time at least 5 minutes past the current time.</span></span> |
| <span data-ttu-id="3e481-223">Upprepning</span><span class="sxs-lookup"><span data-stu-id="3e481-223">Recurrence</span></span> | <span data-ttu-id="3e481-224">Återkommande</span><span class="sxs-lookup"><span data-stu-id="3e481-224">Recurring</span></span> |
| <span data-ttu-id="3e481-225">Upprepas var</span><span class="sxs-lookup"><span data-stu-id="3e481-225">Recur every</span></span> | <span data-ttu-id="3e481-226">1 timme</span><span class="sxs-lookup"><span data-stu-id="3e481-226">1 hour</span></span> |
| <span data-ttu-id="3e481-227">Ange förfallodatum</span><span class="sxs-lookup"><span data-stu-id="3e481-227">Set expiration</span></span> | <span data-ttu-id="3e481-228">Nej</span><span class="sxs-lookup"><span data-stu-id="3e481-228">No</span></span> |

<span data-ttu-id="3e481-229">När schemat skapas måste du ange parametervärden som kommer att användas varje gång det här schemat startar runbook.</span><span class="sxs-lookup"><span data-stu-id="3e481-229">Once the schedule is created, you need to set the parameter values that will be used each time this schedule starts the runbook.</span></span>

6. <span data-ttu-id="3e481-230">Klicka på **konfigurera parametrar och körningsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="3e481-230">Click **Configure parameters and run settings**.</span></span>
7. <span data-ttu-id="3e481-231">Fyll i värden för din **ResourceGroupName** och **AutomationAccountName**.</span><span class="sxs-lookup"><span data-stu-id="3e481-231">Fill in values for your **ResourceGroupName** and **AutomationAccountName**.</span></span>
8. <span data-ttu-id="3e481-232">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3e481-232">Click **OK**.</span></span> 

## <a name="9-verify-runbook-starts-on-schedule"></a><span data-ttu-id="3e481-233">9. Kontrollera runbook startar enligt schema</span><span class="sxs-lookup"><span data-stu-id="3e481-233">9. Verify runbook starts on schedule</span></span>
<span data-ttu-id="3e481-234">Varje gång en runbook startas [ett jobb skapas](../automation/automation-runbook-execution.md) och all utdata som loggas.</span><span class="sxs-lookup"><span data-stu-id="3e481-234">Everytime a runbook is started, [a job is created](../automation/automation-runbook-execution.md) and any output logged.</span></span>  <span data-ttu-id="3e481-235">Faktum är är det här samma jobb som samlar in runbook.</span><span class="sxs-lookup"><span data-stu-id="3e481-235">In fact, these are the same jobs that the runbook is collecting.</span></span>  <span data-ttu-id="3e481-236">Du kan kontrollera att runbook startar som förväntat genom att kontrollera jobb för runbook efter starttiden för schemat har överskridits.</span><span class="sxs-lookup"><span data-stu-id="3e481-236">You can verify that the runbook starts as expected by checking the jobs for the runbook after the start time for the schedule has passed.</span></span>

![Jobb](media/operations-management-suite-runbook-datacollect/jobs.png)

1. <span data-ttu-id="3e481-238">I egenskaperna för din runbook, Välj **jobb** under **resurser**.</span><span class="sxs-lookup"><span data-stu-id="3e481-238">In the properties for your runbook, select **Jobs** under **Resources**.</span></span>
2. <span data-ttu-id="3e481-239">Du bör se en lista över jobb varje gång runbook startades.</span><span class="sxs-lookup"><span data-stu-id="3e481-239">You should see a listing of jobs for each time the runbook was started.</span></span>
3. <span data-ttu-id="3e481-240">Klicka på ett av jobb att visa information.</span><span class="sxs-lookup"><span data-stu-id="3e481-240">Click on one of the jobs to view its details.</span></span>
4. <span data-ttu-id="3e481-241">Klicka på **alla loggar** att visa loggfilerna och utdata från runbook.</span><span class="sxs-lookup"><span data-stu-id="3e481-241">Click on **All logs** to view the logs and output from the runbook.</span></span>
5. <span data-ttu-id="3e481-242">Rulla ned för att hitta en post som liknar bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="3e481-242">Scroll to the bottom to find an entry similar to the image below.</span></span><br>![Utförlig](media/operations-management-suite-runbook-datacollect/verbose.png)
6. <span data-ttu-id="3e481-244">Klicka på den här posten för att visa detaljerad json-data som skickades till logganalys.</span><span class="sxs-lookup"><span data-stu-id="3e481-244">Click on this entry to view the detailed json data  that was sent to Log Analytics.</span></span>



## <a name="next-steps"></a><span data-ttu-id="3e481-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e481-245">Next steps</span></span>
- <span data-ttu-id="3e481-246">Använd [Vydesigner](../log-analytics/log-analytics-view-designer.md) att skapa en vy som visar data som du har lagrat i logganalys-databasen.</span><span class="sxs-lookup"><span data-stu-id="3e481-246">Use [View Designer](../log-analytics/log-analytics-view-designer.md) to create a view displaying the data that you've collected to the Log Analytics repository.</span></span>
- <span data-ttu-id="3e481-247">Paketera din runbook i en [hanteringslösning](operations-management-suite-solutions-creating.md) ska distribueras till kunder.</span><span class="sxs-lookup"><span data-stu-id="3e481-247">Package your runbook in a [management solution](operations-management-suite-solutions-creating.md) to distribute to customers.</span></span>
- <span data-ttu-id="3e481-248">Lär dig mer om [logganalys](https://docs.microsoft.com/azure/log-analytics/).</span><span class="sxs-lookup"><span data-stu-id="3e481-248">Learn more about [Log Analytics](https://docs.microsoft.com/azure/log-analytics/).</span></span>
- <span data-ttu-id="3e481-249">Lär dig mer om [Azure Automation](https://docs.microsoft.com/azure/automation/).</span><span class="sxs-lookup"><span data-stu-id="3e481-249">Learn more about [Azure Automation](https://docs.microsoft.com/azure/automation/).</span></span>
- <span data-ttu-id="3e481-250">Lär dig mer om den [HTTP Data Collector API: et](../log-analytics/log-analytics-data-collector-api.md).</span><span class="sxs-lookup"><span data-stu-id="3e481-250">Learn more about the [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md).</span></span>