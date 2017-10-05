---
title: "Starta och stoppa virtuella datorer med Azure Automation - PowerShell-arbetsflöde | Microsoft Docs"
description: "Grafiska versionen av Azure Automation-scenariot inklusive runbooks för att starta och stoppa klassiska virtuella datorer."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d380bd43-d45d-45af-a5b2-78e7f66263c3
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2016
ms.author: magoedte;bwren
redirect_url: https://docs.microsoft.com/azure/automation/automation-solution-vm-management
redirect_document_id: FALSE
ms.openlocfilehash: 95a7b02b0d11bf18c398daea48d16e0ead30b543
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a><span data-ttu-id="74d86-103">Azure Automation-scenario – starta och stoppa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="74d86-103">Azure Automation scenario - starting and stopping virtual machines</span></span>
<span data-ttu-id="74d86-104">Det här scenariot för Azure Automation innehåller runbooks för att starta och stoppa klassiska virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="74d86-104">This Azure Automation scenario includes runbooks to start and stop classic virtual machines.</span></span>  <span data-ttu-id="74d86-105">Du kan använda det här scenariot för något av följande:</span><span class="sxs-lookup"><span data-stu-id="74d86-105">You can use this scenario for any of the following:</span></span>  

* <span data-ttu-id="74d86-106">Använda runbooks utan ändringar i din egen miljö.</span><span class="sxs-lookup"><span data-stu-id="74d86-106">Use the runbooks without modification in your own environment.</span></span>
* <span data-ttu-id="74d86-107">Ändra runbooks för att utföra anpassade funktioner.</span><span class="sxs-lookup"><span data-stu-id="74d86-107">Modify the runbooks to perform customized functionality.</span></span>  
* <span data-ttu-id="74d86-108">Anropa runbooks från en annan runbook som en del av en övergripande lösning.</span><span class="sxs-lookup"><span data-stu-id="74d86-108">Call the runbooks from another runbook as part of an overall solution.</span></span>
* <span data-ttu-id="74d86-109">Använda runbooks som självstudier för att lära dig runbook authoring begrepp.</span><span class="sxs-lookup"><span data-stu-id="74d86-109">Use the runbooks as tutorials to learn runbook authoring concepts.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="74d86-110">Grafisk</span><span class="sxs-lookup"><span data-stu-id="74d86-110">Graphical</span></span>](automation-solution-startstopvm-graphical.md)
> * [<span data-ttu-id="74d86-111">PowerShell-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="74d86-111">PowerShell Workflow</span></span>](automation-solution-startstopvm-psworkflow.md)
> 
> 

<span data-ttu-id="74d86-112">Detta är PowerShell-arbetsflöde runbook-versionen av det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="74d86-112">This is the PowerShell Workflow runbook version of this scenario.</span></span> <span data-ttu-id="74d86-113">Det är också tillgängliga använder [grafiska runbook-flöden](automation-solution-startstopvm-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="74d86-113">It is also available using [graphical runbooks](automation-solution-startstopvm-graphical.md).</span></span>

## <a name="getting-the-scenario"></a><span data-ttu-id="74d86-114">Hämta scenariot</span><span class="sxs-lookup"><span data-stu-id="74d86-114">Getting the scenario</span></span>
<span data-ttu-id="74d86-115">Det här scenariot består av två PowerShell-arbetsflöde runbooks som kan hämtas från följande länkar.</span><span class="sxs-lookup"><span data-stu-id="74d86-115">This scenario consists of two PowerShell Workflow runbooks that you can download from the following links.</span></span>  <span data-ttu-id="74d86-116">Finns det [grafiska version](automation-solution-startstopvm-graphical.md) i det här scenariot länkar till grafiska runbook-flöden.</span><span class="sxs-lookup"><span data-stu-id="74d86-116">See the [graphical version](automation-solution-startstopvm-graphical.md) of this scenario for links to the graphical runbooks.</span></span>

| <span data-ttu-id="74d86-117">Runbook</span><span class="sxs-lookup"><span data-stu-id="74d86-117">Runbook</span></span> | <span data-ttu-id="74d86-118">Länk</span><span class="sxs-lookup"><span data-stu-id="74d86-118">Link</span></span> | <span data-ttu-id="74d86-119">Typ</span><span class="sxs-lookup"><span data-stu-id="74d86-119">Type</span></span> | <span data-ttu-id="74d86-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74d86-120">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="74d86-121">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="74d86-121">Start-AzureVMs</span></span> |[<span data-ttu-id="74d86-122">Starta klassiska virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="74d86-122">Start Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |<span data-ttu-id="74d86-123">PowerShell-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="74d86-123">PowerShell Workflow</span></span> |<span data-ttu-id="74d86-124">Startar alla klassiska virtuella datorer i en Azure subscriptionor alla virtuella datorer med ett visst namn.</span><span class="sxs-lookup"><span data-stu-id="74d86-124">Starts all classic virtual machines in an Azure subscriptionor all virtual machines with a particular service name.</span></span> |
| <span data-ttu-id="74d86-125">Stoppa AzureVMs</span><span class="sxs-lookup"><span data-stu-id="74d86-125">Stop-AzureVMs</span></span> |[<span data-ttu-id="74d86-126">Stoppa klassiska virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="74d86-126">Stop Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |<span data-ttu-id="74d86-127">PowerShell-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="74d86-127">PowerShell Workflow</span></span> |<span data-ttu-id="74d86-128">Stoppar alla virtuella datorer i ett automation-konto eller alla virtuella datorer med ett visst namn.</span><span class="sxs-lookup"><span data-stu-id="74d86-128">Stops all virtual machines in an automation account or all virtual machines with a particular service name.</span></span> |

## <a name="installing-and-configuring-the-scenario"></a><span data-ttu-id="74d86-129">Installera och konfigurera scenariot</span><span class="sxs-lookup"><span data-stu-id="74d86-129">Installing and configuring the scenario</span></span>
### <a name="1-install-the-runbooks"></a><span data-ttu-id="74d86-130">1. Installera runbooks</span><span class="sxs-lookup"><span data-stu-id="74d86-130">1. Install the runbooks</span></span>
<span data-ttu-id="74d86-131">När du har hämtat runbooks, kan du importera dem med hjälp av proceduren i [importera en Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span><span class="sxs-lookup"><span data-stu-id="74d86-131">After downloading the runbooks, you can import them using the procedure in [Importing a Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span></span>

### <a name="2-review-the-description-and-requirements"></a><span data-ttu-id="74d86-132">2. Granska beskrivning och krav</span><span class="sxs-lookup"><span data-stu-id="74d86-132">2. Review the description and requirements</span></span>
<span data-ttu-id="74d86-133">Runbooks är kommenterade hjälptext som innehåller en beskrivning och nödvändiga tillgångar.</span><span class="sxs-lookup"><span data-stu-id="74d86-133">The runbooks include commented help text that includes a description and required assets.</span></span>  <span data-ttu-id="74d86-134">Du kan också få samma information från den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="74d86-134">You can also get the same information from this article.</span></span>

### <a name="3-configure-assets"></a><span data-ttu-id="74d86-135">3. Konfigurera tillgångar</span><span class="sxs-lookup"><span data-stu-id="74d86-135">3. Configure assets</span></span>
<span data-ttu-id="74d86-136">Runbooks kräver följande resurser som du måste skapa och fylla i med lämpliga värden.</span><span class="sxs-lookup"><span data-stu-id="74d86-136">The runbooks require the following assets that you must create and populate with appropriate values.</span></span>

| <span data-ttu-id="74d86-137">Tillgångstypen</span><span class="sxs-lookup"><span data-stu-id="74d86-137">Asset Type</span></span> | <span data-ttu-id="74d86-138">Tillgångsnamnet</span><span class="sxs-lookup"><span data-stu-id="74d86-138">Asset Name</span></span> | <span data-ttu-id="74d86-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74d86-139">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="74d86-140">Autentiseringsuppgift</span><span class="sxs-lookup"><span data-stu-id="74d86-140">Credential</span></span> |<span data-ttu-id="74d86-141">AzureCredential</span><span class="sxs-lookup"><span data-stu-id="74d86-141">AzureCredential</span></span> |<span data-ttu-id="74d86-142">Innehåller autentiseringsuppgifter för ett konto som har behörighet att starta och stoppa virtuella datorer i Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="74d86-142">Contains credentials for an account that has authority to start and stop virtual machines in the Azure subscription.</span></span>  <span data-ttu-id="74d86-143">Du kan också ange en annan autentiseringsuppgiftstillgång i den **autentiseringsuppgifter** parameter för den **Add-AzureAccount** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="74d86-143">Alternatively, you can specify another credential asset in the **Credential** parameter of the **Add-AzureAccount** activity.</span></span> |
| <span data-ttu-id="74d86-144">Variabel</span><span class="sxs-lookup"><span data-stu-id="74d86-144">Variable</span></span> |<span data-ttu-id="74d86-145">AzureSubscriptionId</span><span class="sxs-lookup"><span data-stu-id="74d86-145">AzureSubscriptionId</span></span> |<span data-ttu-id="74d86-146">Innehåller prenumerations-ID för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="74d86-146">Contains the subscription ID of your Azure subscription.</span></span> |

## <a name="using-the-scenario"></a><span data-ttu-id="74d86-147">Med scenario</span><span class="sxs-lookup"><span data-stu-id="74d86-147">Using the scenario</span></span>
### <a name="parameters"></a><span data-ttu-id="74d86-148">Parametrar</span><span class="sxs-lookup"><span data-stu-id="74d86-148">Parameters</span></span>
<span data-ttu-id="74d86-149">Runbooks har följande parametrar.</span><span class="sxs-lookup"><span data-stu-id="74d86-149">The runbooks each have the following parameters.</span></span>  <span data-ttu-id="74d86-150">Du måste ange värden för alla obligatoriska parametrar och kan du ange värden för andra parametrar beroende på dina krav.</span><span class="sxs-lookup"><span data-stu-id="74d86-150">You must provide values for any mandatory parameters and can optionally provide values for other parameters depending on your requirements.</span></span>

| <span data-ttu-id="74d86-151">Parameter</span><span class="sxs-lookup"><span data-stu-id="74d86-151">Parameter</span></span> | <span data-ttu-id="74d86-152">Typ</span><span class="sxs-lookup"><span data-stu-id="74d86-152">Type</span></span> | <span data-ttu-id="74d86-153">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="74d86-153">Mandatory</span></span> | <span data-ttu-id="74d86-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74d86-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="74d86-155">Tjänstnamn</span><span class="sxs-lookup"><span data-stu-id="74d86-155">ServiceName</span></span> |<span data-ttu-id="74d86-156">Sträng</span><span class="sxs-lookup"><span data-stu-id="74d86-156">string</span></span> |<span data-ttu-id="74d86-157">Nej</span><span class="sxs-lookup"><span data-stu-id="74d86-157">No</span></span> |<span data-ttu-id="74d86-158">Om ett värde anges, och sedan alla virtuella datorer med samma tjänstnamn startas eller stoppas.</span><span class="sxs-lookup"><span data-stu-id="74d86-158">If a value is provided, then all virtual machines with that service name are started or stopped.</span></span>  <span data-ttu-id="74d86-159">Om inget värde anges, är sedan alla klassiska virtuella datorer i Azure-prenumerationen startas eller stoppas.</span><span class="sxs-lookup"><span data-stu-id="74d86-159">If no value is provided, then all classic virtual machines in the Azure subscription are started or stopped.</span></span> |
| <span data-ttu-id="74d86-160">AzureSubscriptionIdAssetName</span><span class="sxs-lookup"><span data-stu-id="74d86-160">AzureSubscriptionIdAssetName</span></span> |<span data-ttu-id="74d86-161">Sträng</span><span class="sxs-lookup"><span data-stu-id="74d86-161">string</span></span> |<span data-ttu-id="74d86-162">Nej</span><span class="sxs-lookup"><span data-stu-id="74d86-162">No</span></span> |<span data-ttu-id="74d86-163">Innehåller namnet på den [variabeltillgång](#installing-and-configuring-the-scenario) som innehåller prenumerations-ID för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="74d86-163">Contains the name of the [variable asset](#installing-and-configuring-the-scenario) that contains the subscription ID of your Azure subscription.</span></span>  <span data-ttu-id="74d86-164">Om du inte anger ett värde, *AzureSubscriptionId* används.</span><span class="sxs-lookup"><span data-stu-id="74d86-164">If you don't specify a value, *AzureSubscriptionId* is used.</span></span> |
| <span data-ttu-id="74d86-165">AzureCredentialAssetName</span><span class="sxs-lookup"><span data-stu-id="74d86-165">AzureCredentialAssetName</span></span> |<span data-ttu-id="74d86-166">Sträng</span><span class="sxs-lookup"><span data-stu-id="74d86-166">string</span></span> |<span data-ttu-id="74d86-167">Nej</span><span class="sxs-lookup"><span data-stu-id="74d86-167">No</span></span> |<span data-ttu-id="74d86-168">Innehåller namnet på den [autentiseringsuppgiftstillgång](#installing-and-configuring-the-scenario) som innehåller autentiseringsuppgifter för runbook ska användas.</span><span class="sxs-lookup"><span data-stu-id="74d86-168">Contains the name of the [credential asset](#installing-and-configuring-the-scenario) that contains the credentials for the runbook to use.</span></span>  <span data-ttu-id="74d86-169">Om du inte anger ett värde, *AzureCredential* används.</span><span class="sxs-lookup"><span data-stu-id="74d86-169">If you don't specify a value, *AzureCredential* is used.</span></span> |

### <a name="starting-the-runbooks"></a><span data-ttu-id="74d86-170">Starta runbooks</span><span class="sxs-lookup"><span data-stu-id="74d86-170">Starting the runbooks</span></span>
<span data-ttu-id="74d86-171">Du kan använda någon av metoderna i [starta en runbook i Azure Automation](automation-starting-a-runbook.md) starta antingen runbooks i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="74d86-171">You can use any of the methods in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) to start either of the runbooks in this scenario.</span></span>

<span data-ttu-id="74d86-172">Följande exempelkommandon använder Windows PowerShell för att köra **StartAzureVMs** starta alla virtuella datorer med tjänstnamnet *MyVMService*.</span><span class="sxs-lookup"><span data-stu-id="74d86-172">The following sample commands uses Windows PowerShell to run **StartAzureVMs** to start all virtual machines with the service name *MyVMService*.</span></span>

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a><span data-ttu-id="74d86-173">Resultat</span><span class="sxs-lookup"><span data-stu-id="74d86-173">Output</span></span>
<span data-ttu-id="74d86-174">Runbooks kommer [utdata meddelandet](automation-runbook-output-and-messages.md) för varje virtuell dator som anger huruvida starta eller stoppa instruktion skickades.</span><span class="sxs-lookup"><span data-stu-id="74d86-174">The runbooks will [output a message](automation-runbook-output-and-messages.md) for each virtual machine indicating whether or not the start or stop instruction was successfully submitted.</span></span>  <span data-ttu-id="74d86-175">Du kan söka efter en specifik sträng i utdata att fastställa resultatet för varje runbook.</span><span class="sxs-lookup"><span data-stu-id="74d86-175">You can look for a specific string in the output to determine the result for each runbook.</span></span>  <span data-ttu-id="74d86-176">I följande tabell visas eventuella utdata-strängar.</span><span class="sxs-lookup"><span data-stu-id="74d86-176">The possible output strings are listed in the following table.</span></span>

| <span data-ttu-id="74d86-177">Runbook</span><span class="sxs-lookup"><span data-stu-id="74d86-177">Runbook</span></span> | <span data-ttu-id="74d86-178">Villkor</span><span class="sxs-lookup"><span data-stu-id="74d86-178">Condition</span></span> | <span data-ttu-id="74d86-179">Meddelande</span><span class="sxs-lookup"><span data-stu-id="74d86-179">Message</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="74d86-180">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="74d86-180">Start-AzureVMs</span></span> |<span data-ttu-id="74d86-181">Virtuell dator körs redan</span><span class="sxs-lookup"><span data-stu-id="74d86-181">Virtual machine is already running</span></span> |<span data-ttu-id="74d86-182">MyVM körs redan</span><span class="sxs-lookup"><span data-stu-id="74d86-182">MyVM is already running</span></span> |
| <span data-ttu-id="74d86-183">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="74d86-183">Start-AzureVMs</span></span> |<span data-ttu-id="74d86-184">Start-begäran för den virtuella datorn har skickats</span><span class="sxs-lookup"><span data-stu-id="74d86-184">Start request for virtual machine successfully submitted</span></span> |<span data-ttu-id="74d86-185">MyVM har startats</span><span class="sxs-lookup"><span data-stu-id="74d86-185">MyVM has been started</span></span> |
| <span data-ttu-id="74d86-186">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="74d86-186">Start-AzureVMs</span></span> |<span data-ttu-id="74d86-187">Det gick inte att startbegäran för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="74d86-187">Start request for virtual machine failed</span></span> |<span data-ttu-id="74d86-188">Det gick inte att starta MyVM</span><span class="sxs-lookup"><span data-stu-id="74d86-188">MyVM failed to start</span></span> |
| <span data-ttu-id="74d86-189">Stoppa AzureVMs</span><span class="sxs-lookup"><span data-stu-id="74d86-189">Stop-AzureVMs</span></span> |<span data-ttu-id="74d86-190">Virtuell dator har redan stoppats.</span><span class="sxs-lookup"><span data-stu-id="74d86-190">Virtual machine is already stopped</span></span> |<span data-ttu-id="74d86-191">MyVM har redan stoppats.</span><span class="sxs-lookup"><span data-stu-id="74d86-191">MyVM is already stopped</span></span> |
| <span data-ttu-id="74d86-192">Stoppa AzureVMs</span><span class="sxs-lookup"><span data-stu-id="74d86-192">Stop-AzureVMs</span></span> |<span data-ttu-id="74d86-193">Stoppa begäran för den virtuella datorn har skickats</span><span class="sxs-lookup"><span data-stu-id="74d86-193">Stop request for virtual machine successfully submitted</span></span> |<span data-ttu-id="74d86-194">MyVM har stoppats</span><span class="sxs-lookup"><span data-stu-id="74d86-194">MyVM has been stopped</span></span> |
| <span data-ttu-id="74d86-195">Stoppa AzureVMs</span><span class="sxs-lookup"><span data-stu-id="74d86-195">Stop-AzureVMs</span></span> |<span data-ttu-id="74d86-196">Stop-begäran för den virtuella datorn misslyckades</span><span class="sxs-lookup"><span data-stu-id="74d86-196">Stop request for virtual machine failed</span></span> |<span data-ttu-id="74d86-197">Det gick inte att stoppa MyVM</span><span class="sxs-lookup"><span data-stu-id="74d86-197">MyVM failed to stop</span></span> |

<span data-ttu-id="74d86-198">Till exempel följande kodavsnitt från en runbook försöker starta alla virtuella datorer med tjänstnamnet *MyServiceName*.</span><span class="sxs-lookup"><span data-stu-id="74d86-198">For example, the following code snippet from a runbook attempts to start all virtual machines with the service name *MyServiceName*.</span></span>  <span data-ttu-id="74d86-199">Om någon av start-begäranden misslyckas, kan åtgärder vidtas.</span><span class="sxs-lookup"><span data-stu-id="74d86-199">If any of the start requests fail, then error actions can be taken.</span></span>

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a><span data-ttu-id="74d86-200">Detaljerad analys</span><span class="sxs-lookup"><span data-stu-id="74d86-200">Detailed breakdown</span></span>
<span data-ttu-id="74d86-201">Nedan följer en detaljerad analys av runbooks i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="74d86-201">Following is a detailed breakdown of the runbooks in this scenario.</span></span>  <span data-ttu-id="74d86-202">Du kan använda den här informationen för att anpassa runbooks eller bara om du vill veta från dem för att skapa egna automatiseringsscenarier.</span><span class="sxs-lookup"><span data-stu-id="74d86-202">You can use this information to either customize the runbooks or just to learn from them for authoring your own automation scenarios.</span></span>

### <a name="parameters"></a><span data-ttu-id="74d86-203">Parametrar</span><span class="sxs-lookup"><span data-stu-id="74d86-203">Parameters</span></span>
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

<span data-ttu-id="74d86-204">Arbetsflödet startar med att hämta värden för den [indataparametrar](#using-the-scenario).</span><span class="sxs-lookup"><span data-stu-id="74d86-204">The workflow starts by getting the values for the [input parameters](#using-the-scenario).</span></span>  <span data-ttu-id="74d86-205">Om de tillgången inte anges används standardnamn.</span><span class="sxs-lookup"><span data-stu-id="74d86-205">If the asset names are not provided then default names are used.</span></span>

### <a name="output"></a><span data-ttu-id="74d86-206">Resultat</span><span class="sxs-lookup"><span data-stu-id="74d86-206">Output</span></span>
    # Returns strings with status messages
    [OutputType([String])]

<span data-ttu-id="74d86-207">Den här raden deklarerar att utdata från runbooken ska vara en sträng.</span><span class="sxs-lookup"><span data-stu-id="74d86-207">This line declares that the output of the runbook will be a string.</span></span>  <span data-ttu-id="74d86-208">Detta är inte obligatoriskt men det är bästa praxis för när runbook används som en [underordnad runbook](automation-child-runbooks.md) så att en överordnad runbook vet utdatatypen förväntar dig.</span><span class="sxs-lookup"><span data-stu-id="74d86-208">This is not required but is a best practice for when the runbook is used as a [child runbook](automation-child-runbooks.md) so that a parent runbook will know the output type to expect.</span></span>

### <a name="authentication"></a><span data-ttu-id="74d86-209">Autentisering</span><span class="sxs-lookup"><span data-stu-id="74d86-209">Authentication</span></span>
    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

<span data-ttu-id="74d86-210">Nästa rader uppsättningen av [autentiseringsuppgifter](automation-credentials.md) och Azure-prenumeration som ska användas för resten av runbook.</span><span class="sxs-lookup"><span data-stu-id="74d86-210">The next lines set the [credentials](automation-credentials.md) and Azure subscription that will be used for the rest of the runbook.</span></span>
<span data-ttu-id="74d86-211">Första vi använder **Get-AutomationPSCredential** att hämta den tillgång som innehåller autentiseringsuppgifter för åtkomst till starta och stoppa virtuella datorer i Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="74d86-211">First we use **Get-AutomationPSCredential** to get the asset that holds the credentials with access to start and stop virtual machines in the Azure subscription.</span></span> <span data-ttu-id="74d86-212">**Lägg till AzureAccount** använder sedan tillgången för att ange autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="74d86-212">**Add-AzureAccount** then uses this asset to set the credentials.</span></span>  <span data-ttu-id="74d86-213">Utdata har tilldelats en dummy variabel så att den inte ingår i runbook-utdata.</span><span class="sxs-lookup"><span data-stu-id="74d86-213">The output is assigned to a dummy variable so that it isn't included in the runbook output.</span></span>  

<span data-ttu-id="74d86-214">Variabeltillgång med ID: T hämtas med prenumerationen **Get-automationvariable,** och prenumerationen med **Välj AzureSubscription**.</span><span class="sxs-lookup"><span data-stu-id="74d86-214">The variable asset with the subscription ID is then retrieved with **Get-AutomationVariable** and the subscription set with **Select-AzureSubscription**.</span></span>

### <a name="get-vms"></a><span data-ttu-id="74d86-215">Hämta virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="74d86-215">Get VMs</span></span>
    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

<span data-ttu-id="74d86-216">**Get-AzureVM** används för att hämta de virtuella datorerna runbook fungerar med.</span><span class="sxs-lookup"><span data-stu-id="74d86-216">**Get-AzureVM** is used to retrieve the virtual machines the runbook will work with.</span></span>  <span data-ttu-id="74d86-217">Om ett värde har angetts i den **ServiceName** ange variabel, och sedan på virtuella datorer med att namn har hämtats.</span><span class="sxs-lookup"><span data-stu-id="74d86-217">If a value is provided in the **ServiceName** input variable, then only the virtual machines with that service name are retrieved.</span></span>  <span data-ttu-id="74d86-218">Om **ServiceName** är tom, hämtas alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="74d86-218">If **ServiceName** is empty, then all virtual machines are retrieved.</span></span>

### <a name="startstop-virtual-machines-and-send-output"></a><span data-ttu-id="74d86-219">Starta/stoppa virtuella datorer och skicka utdata</span><span class="sxs-lookup"><span data-stu-id="74d86-219">Start/Stop virtual machines and send output</span></span>
    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

<span data-ttu-id="74d86-220">På nästa rad gå igenom varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="74d86-220">The next lines step through each virtual machine.</span></span>  <span data-ttu-id="74d86-221">Första den **PowerState** för den virtuella datorn kontrolleras för att se om den redan är igång eller Stoppad, beroende på runbook.</span><span class="sxs-lookup"><span data-stu-id="74d86-221">First the **PowerState** of the virtual machine is checked to see if it is already running or stopped, depending on the runbook.</span></span>  <span data-ttu-id="74d86-222">Om det redan finns i måltillståndet skickas ett meddelande till utdata och runbook avslutas.</span><span class="sxs-lookup"><span data-stu-id="74d86-222">If it is already in the target state, then a message is sent to output, and the runbook ends.</span></span>  <span data-ttu-id="74d86-223">Om inte, sedan **Start AzureVM** eller **stoppa AzureVM** används för att försöka starta eller stoppa den virtuella datorn med resultatet av begäran lagras i en variabel.</span><span class="sxs-lookup"><span data-stu-id="74d86-223">If not, then **Start-AzureVM** or **Stop-AzureVM** is used to attempt to start or stop the virtual machine with the result of the request stored to a variable.</span></span>  <span data-ttu-id="74d86-224">Ett meddelande skickas sedan till utdata anger om begäran om att starta eller stoppa har skickats.</span><span class="sxs-lookup"><span data-stu-id="74d86-224">A message is then sent to output specifying whether the request to start or stop was submitted successfully.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74d86-225">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74d86-225">Next steps</span></span>
* <span data-ttu-id="74d86-226">Mer information om hur du arbetar med underordnade runbooks finns [underordnade runbooks i Azure Automation](automation-child-runbooks.md)</span><span class="sxs-lookup"><span data-stu-id="74d86-226">To learn more about working with child runbooks, see [Child runbooks in Azure Automation](automation-child-runbooks.md)</span></span>
* <span data-ttu-id="74d86-227">Läs mer om utgående meddelanden under körning av runbook och loggning för att felsöka i [Runbook-utdata och meddelanden i Azure Automation](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="74d86-227">To learn more about output messages during runbook execution and logging to help troubleshoot, see [Runbook output and messages in Azure Automation](automation-runbook-output-and-messages.md)</span></span>

