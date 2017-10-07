---
title: "aaaStarting och stoppa virtuella datorer med Azure Automation - PowerShell-arbetsflöde | Microsoft Docs"
description: Grafiska versionen av Azure Automation-scenariot inklusive runbooks toostart och stoppa klassiska virtuella datorer.
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
redirect_document_id: False
ms.openlocfilehash: 273631c7fc5ddb989b3bbdc82b470ac3af6ee482
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a><span data-ttu-id="db71d-103">Azure Automation-scenario – starta och stoppa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="db71d-103">Azure Automation scenario - starting and stopping virtual machines</span></span>
<span data-ttu-id="db71d-104">Det här scenariot för Azure Automation innehåller runbooks toostart och stoppa klassiska virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="db71d-104">This Azure Automation scenario includes runbooks toostart and stop classic virtual machines.</span></span>  <span data-ttu-id="db71d-105">Du kan använda det här scenariot för hello följande:</span><span class="sxs-lookup"><span data-stu-id="db71d-105">You can use this scenario for any of hello following:</span></span>  

* <span data-ttu-id="db71d-106">Använd hello runbooks utan ändringar i din egen miljö.</span><span class="sxs-lookup"><span data-stu-id="db71d-106">Use hello runbooks without modification in your own environment.</span></span>
* <span data-ttu-id="db71d-107">Ändra hello runbooks tooperform anpassade funktioner.</span><span class="sxs-lookup"><span data-stu-id="db71d-107">Modify hello runbooks tooperform customized functionality.</span></span>  
* <span data-ttu-id="db71d-108">Anropa hello runbooks från en annan runbook som en del av en övergripande lösning.</span><span class="sxs-lookup"><span data-stu-id="db71d-108">Call hello runbooks from another runbook as part of an overall solution.</span></span>
* <span data-ttu-id="db71d-109">Använd hello runbooks som självstudier toolearn runbook authoring begrepp.</span><span class="sxs-lookup"><span data-stu-id="db71d-109">Use hello runbooks as tutorials toolearn runbook authoring concepts.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="db71d-110">Grafisk</span><span class="sxs-lookup"><span data-stu-id="db71d-110">Graphical</span></span>](automation-solution-startstopvm-graphical.md)
> * [<span data-ttu-id="db71d-111">PowerShell-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="db71d-111">PowerShell Workflow</span></span>](automation-solution-startstopvm-psworkflow.md)
> 
> 

<span data-ttu-id="db71d-112">Detta är hello PowerShell-arbetsflöde runbook-versionen av det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="db71d-112">This is hello PowerShell Workflow runbook version of this scenario.</span></span> <span data-ttu-id="db71d-113">Det är också tillgängliga använder [grafiska runbook-flöden](automation-solution-startstopvm-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="db71d-113">It is also available using [graphical runbooks](automation-solution-startstopvm-graphical.md).</span></span>

## <a name="getting-hello-scenario"></a><span data-ttu-id="db71d-114">Hämta hello scenario</span><span class="sxs-lookup"><span data-stu-id="db71d-114">Getting hello scenario</span></span>
<span data-ttu-id="db71d-115">Det här scenariot består av två runbooks i PowerShell-arbetsflöde som du kan hämta från hello följande länkar.</span><span class="sxs-lookup"><span data-stu-id="db71d-115">This scenario consists of two PowerShell Workflow runbooks that you can download from hello following links.</span></span>  <span data-ttu-id="db71d-116">Se hello [grafiska version](automation-solution-startstopvm-graphical.md) i det här scenariot för länkar toohello grafiska runbook-flöden.</span><span class="sxs-lookup"><span data-stu-id="db71d-116">See hello [graphical version](automation-solution-startstopvm-graphical.md) of this scenario for links toohello graphical runbooks.</span></span>

| <span data-ttu-id="db71d-117">Runbook</span><span class="sxs-lookup"><span data-stu-id="db71d-117">Runbook</span></span> | <span data-ttu-id="db71d-118">Länk</span><span class="sxs-lookup"><span data-stu-id="db71d-118">Link</span></span> | <span data-ttu-id="db71d-119">Typ</span><span class="sxs-lookup"><span data-stu-id="db71d-119">Type</span></span> | <span data-ttu-id="db71d-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="db71d-120">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="db71d-121">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="db71d-121">Start-AzureVMs</span></span> |[<span data-ttu-id="db71d-122">Starta klassiska virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="db71d-122">Start Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |<span data-ttu-id="db71d-123">PowerShell-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="db71d-123">PowerShell Workflow</span></span> |<span data-ttu-id="db71d-124">Startar alla klassiska virtuella datorer i en Azure subscriptionor alla virtuella datorer med ett visst namn.</span><span class="sxs-lookup"><span data-stu-id="db71d-124">Starts all classic virtual machines in an Azure subscriptionor all virtual machines with a particular service name.</span></span> |
| <span data-ttu-id="db71d-125">Stoppa AzureVMs</span><span class="sxs-lookup"><span data-stu-id="db71d-125">Stop-AzureVMs</span></span> |[<span data-ttu-id="db71d-126">Stoppa klassiska virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="db71d-126">Stop Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |<span data-ttu-id="db71d-127">PowerShell-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="db71d-127">PowerShell Workflow</span></span> |<span data-ttu-id="db71d-128">Stoppar alla virtuella datorer i ett automation-konto eller alla virtuella datorer med ett visst namn.</span><span class="sxs-lookup"><span data-stu-id="db71d-128">Stops all virtual machines in an automation account or all virtual machines with a particular service name.</span></span> |

## <a name="installing-and-configuring-hello-scenario"></a><span data-ttu-id="db71d-129">Installera och konfigurera hello scenario</span><span class="sxs-lookup"><span data-stu-id="db71d-129">Installing and configuring hello scenario</span></span>
### <a name="1-install-hello-runbooks"></a><span data-ttu-id="db71d-130">1. Installera hello runbooks</span><span class="sxs-lookup"><span data-stu-id="db71d-130">1. Install hello runbooks</span></span>
<span data-ttu-id="db71d-131">När du har hämtat hello runbooks, kan du importera dem med hjälp av proceduren hello i [importera en Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span><span class="sxs-lookup"><span data-stu-id="db71d-131">After downloading hello runbooks, you can import them using hello procedure in [Importing a Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span></span>

### <a name="2-review-hello-description-and-requirements"></a><span data-ttu-id="db71d-132">2. Granska hello beskrivning och krav</span><span class="sxs-lookup"><span data-stu-id="db71d-132">2. Review hello description and requirements</span></span>
<span data-ttu-id="db71d-133">Hej runbooks är kommenterade hjälptext som innehåller en beskrivning och nödvändiga tillgångar.</span><span class="sxs-lookup"><span data-stu-id="db71d-133">hello runbooks include commented help text that includes a description and required assets.</span></span>  <span data-ttu-id="db71d-134">Du kan också få hello samma information från den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="db71d-134">You can also get hello same information from this article.</span></span>

### <a name="3-configure-assets"></a><span data-ttu-id="db71d-135">3. Konfigurera tillgångar</span><span class="sxs-lookup"><span data-stu-id="db71d-135">3. Configure assets</span></span>
<span data-ttu-id="db71d-136">Hej runbooks kräver hello efter resurser som du måste skapa och fylla i med lämpliga värden.</span><span class="sxs-lookup"><span data-stu-id="db71d-136">hello runbooks require hello following assets that you must create and populate with appropriate values.</span></span>

| <span data-ttu-id="db71d-137">Tillgångstypen</span><span class="sxs-lookup"><span data-stu-id="db71d-137">Asset Type</span></span> | <span data-ttu-id="db71d-138">Tillgångsnamnet</span><span class="sxs-lookup"><span data-stu-id="db71d-138">Asset Name</span></span> | <span data-ttu-id="db71d-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="db71d-139">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="db71d-140">Autentiseringsuppgift</span><span class="sxs-lookup"><span data-stu-id="db71d-140">Credential</span></span> |<span data-ttu-id="db71d-141">AzureCredential</span><span class="sxs-lookup"><span data-stu-id="db71d-141">AzureCredential</span></span> |<span data-ttu-id="db71d-142">Innehåller autentiseringsuppgifter för ett konto som har behörighet toostart och stoppa virtuella datorer i hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="db71d-142">Contains credentials for an account that has authority toostart and stop virtual machines in hello Azure subscription.</span></span>  <span data-ttu-id="db71d-143">Du kan också ange en annan autentiseringsuppgiftstillgång i hello **autentiseringsuppgifter** parametern för hello **Add-AzureAccount** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="db71d-143">Alternatively, you can specify another credential asset in hello **Credential** parameter of hello **Add-AzureAccount** activity.</span></span> |
| <span data-ttu-id="db71d-144">Variabel</span><span class="sxs-lookup"><span data-stu-id="db71d-144">Variable</span></span> |<span data-ttu-id="db71d-145">AzureSubscriptionId</span><span class="sxs-lookup"><span data-stu-id="db71d-145">AzureSubscriptionId</span></span> |<span data-ttu-id="db71d-146">Innehåller hello prenumerations-ID för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="db71d-146">Contains hello subscription ID of your Azure subscription.</span></span> |

## <a name="using-hello-scenario"></a><span data-ttu-id="db71d-147">Med hello scenariot</span><span class="sxs-lookup"><span data-stu-id="db71d-147">Using hello scenario</span></span>
### <a name="parameters"></a><span data-ttu-id="db71d-148">Parametrar</span><span class="sxs-lookup"><span data-stu-id="db71d-148">Parameters</span></span>
<span data-ttu-id="db71d-149">Hej runbooks har hello följande parametrar.</span><span class="sxs-lookup"><span data-stu-id="db71d-149">hello runbooks each have hello following parameters.</span></span>  <span data-ttu-id="db71d-150">Du måste ange värden för alla obligatoriska parametrar och kan du ange värden för andra parametrar beroende på dina krav.</span><span class="sxs-lookup"><span data-stu-id="db71d-150">You must provide values for any mandatory parameters and can optionally provide values for other parameters depending on your requirements.</span></span>

| <span data-ttu-id="db71d-151">Parameter</span><span class="sxs-lookup"><span data-stu-id="db71d-151">Parameter</span></span> | <span data-ttu-id="db71d-152">Typ</span><span class="sxs-lookup"><span data-stu-id="db71d-152">Type</span></span> | <span data-ttu-id="db71d-153">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="db71d-153">Mandatory</span></span> | <span data-ttu-id="db71d-154">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="db71d-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="db71d-155">Tjänstnamn</span><span class="sxs-lookup"><span data-stu-id="db71d-155">ServiceName</span></span> |<span data-ttu-id="db71d-156">Sträng</span><span class="sxs-lookup"><span data-stu-id="db71d-156">string</span></span> |<span data-ttu-id="db71d-157">Nej</span><span class="sxs-lookup"><span data-stu-id="db71d-157">No</span></span> |<span data-ttu-id="db71d-158">Om ett värde anges, och sedan alla virtuella datorer med samma tjänstnamn startas eller stoppas.</span><span class="sxs-lookup"><span data-stu-id="db71d-158">If a value is provided, then all virtual machines with that service name are started or stopped.</span></span>  <span data-ttu-id="db71d-159">Om inget värde anges, sedan alla klassiska virtuella datorer i hello Azure-prenumeration startas eller stoppas.</span><span class="sxs-lookup"><span data-stu-id="db71d-159">If no value is provided, then all classic virtual machines in hello Azure subscription are started or stopped.</span></span> |
| <span data-ttu-id="db71d-160">AzureSubscriptionIdAssetName</span><span class="sxs-lookup"><span data-stu-id="db71d-160">AzureSubscriptionIdAssetName</span></span> |<span data-ttu-id="db71d-161">Sträng</span><span class="sxs-lookup"><span data-stu-id="db71d-161">string</span></span> |<span data-ttu-id="db71d-162">Nej</span><span class="sxs-lookup"><span data-stu-id="db71d-162">No</span></span> |<span data-ttu-id="db71d-163">Innehåller hello namnet på hello [variabeltillgång](#installing-and-configuring-the-scenario) som innehåller hello prenumerations-ID för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="db71d-163">Contains hello name of hello [variable asset](#installing-and-configuring-the-scenario) that contains hello subscription ID of your Azure subscription.</span></span>  <span data-ttu-id="db71d-164">Om du inte anger ett värde, *AzureSubscriptionId* används.</span><span class="sxs-lookup"><span data-stu-id="db71d-164">If you don't specify a value, *AzureSubscriptionId* is used.</span></span> |
| <span data-ttu-id="db71d-165">AzureCredentialAssetName</span><span class="sxs-lookup"><span data-stu-id="db71d-165">AzureCredentialAssetName</span></span> |<span data-ttu-id="db71d-166">Sträng</span><span class="sxs-lookup"><span data-stu-id="db71d-166">string</span></span> |<span data-ttu-id="db71d-167">Nej</span><span class="sxs-lookup"><span data-stu-id="db71d-167">No</span></span> |<span data-ttu-id="db71d-168">Innehåller hello namnet på hello [autentiseringsuppgiftstillgång](#installing-and-configuring-the-scenario) som innehåller hello autentiseringsuppgifter för hello runbook toouse.</span><span class="sxs-lookup"><span data-stu-id="db71d-168">Contains hello name of hello [credential asset](#installing-and-configuring-the-scenario) that contains hello credentials for hello runbook toouse.</span></span>  <span data-ttu-id="db71d-169">Om du inte anger ett värde, *AzureCredential* används.</span><span class="sxs-lookup"><span data-stu-id="db71d-169">If you don't specify a value, *AzureCredential* is used.</span></span> |

### <a name="starting-hello-runbooks"></a><span data-ttu-id="db71d-170">Starta hello runbooks</span><span class="sxs-lookup"><span data-stu-id="db71d-170">Starting hello runbooks</span></span>
<span data-ttu-id="db71d-171">Du kan använda någon av hello metoder i [starta en runbook i Azure Automation](automation-starting-a-runbook.md) toostart antingen hello runbooks i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="db71d-171">You can use any of hello methods in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) toostart either of hello runbooks in this scenario.</span></span>

<span data-ttu-id="db71d-172">följande exempelkommandon hello använder Windows PowerShell toorun **StartAzureVMs** toostart alla virtuella datorer med hello tjänstnamnet *MyVMService*.</span><span class="sxs-lookup"><span data-stu-id="db71d-172">hello following sample commands uses Windows PowerShell toorun **StartAzureVMs** toostart all virtual machines with hello service name *MyVMService*.</span></span>

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a><span data-ttu-id="db71d-173">Resultat</span><span class="sxs-lookup"><span data-stu-id="db71d-173">Output</span></span>
<span data-ttu-id="db71d-174">Hej runbooks kommer [utdata meddelandet](automation-runbook-output-and-messages.md) för varje virtuell dator som anger huruvida hello starta eller stoppa instruktion har skickats.</span><span class="sxs-lookup"><span data-stu-id="db71d-174">hello runbooks will [output a message](automation-runbook-output-and-messages.md) for each virtual machine indicating whether or not hello start or stop instruction was successfully submitted.</span></span>  <span data-ttu-id="db71d-175">Du kan söka efter en specifik sträng i hello utgående toodetermine hello resultatet för varje runbook.</span><span class="sxs-lookup"><span data-stu-id="db71d-175">You can look for a specific string in hello output toodetermine hello result for each runbook.</span></span>  <span data-ttu-id="db71d-176">hello eventuella utdatasträngar visas i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="db71d-176">hello possible output strings are listed in hello following table.</span></span>

| <span data-ttu-id="db71d-177">Runbook</span><span class="sxs-lookup"><span data-stu-id="db71d-177">Runbook</span></span> | <span data-ttu-id="db71d-178">Villkor</span><span class="sxs-lookup"><span data-stu-id="db71d-178">Condition</span></span> | <span data-ttu-id="db71d-179">Meddelande</span><span class="sxs-lookup"><span data-stu-id="db71d-179">Message</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="db71d-180">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="db71d-180">Start-AzureVMs</span></span> |<span data-ttu-id="db71d-181">Virtuell dator körs redan</span><span class="sxs-lookup"><span data-stu-id="db71d-181">Virtual machine is already running</span></span> |<span data-ttu-id="db71d-182">MyVM körs redan</span><span class="sxs-lookup"><span data-stu-id="db71d-182">MyVM is already running</span></span> |
| <span data-ttu-id="db71d-183">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="db71d-183">Start-AzureVMs</span></span> |<span data-ttu-id="db71d-184">Start-begäran för den virtuella datorn har skickats</span><span class="sxs-lookup"><span data-stu-id="db71d-184">Start request for virtual machine successfully submitted</span></span> |<span data-ttu-id="db71d-185">MyVM har startats</span><span class="sxs-lookup"><span data-stu-id="db71d-185">MyVM has been started</span></span> |
| <span data-ttu-id="db71d-186">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="db71d-186">Start-AzureVMs</span></span> |<span data-ttu-id="db71d-187">Det gick inte att startbegäran för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="db71d-187">Start request for virtual machine failed</span></span> |<span data-ttu-id="db71d-188">MyVM misslyckades toostart</span><span class="sxs-lookup"><span data-stu-id="db71d-188">MyVM failed toostart</span></span> |
| <span data-ttu-id="db71d-189">Stoppa AzureVMs</span><span class="sxs-lookup"><span data-stu-id="db71d-189">Stop-AzureVMs</span></span> |<span data-ttu-id="db71d-190">Virtuell dator har redan stoppats.</span><span class="sxs-lookup"><span data-stu-id="db71d-190">Virtual machine is already stopped</span></span> |<span data-ttu-id="db71d-191">MyVM har redan stoppats.</span><span class="sxs-lookup"><span data-stu-id="db71d-191">MyVM is already stopped</span></span> |
| <span data-ttu-id="db71d-192">Stoppa AzureVMs</span><span class="sxs-lookup"><span data-stu-id="db71d-192">Stop-AzureVMs</span></span> |<span data-ttu-id="db71d-193">Stoppa begäran för den virtuella datorn har skickats</span><span class="sxs-lookup"><span data-stu-id="db71d-193">Stop request for virtual machine successfully submitted</span></span> |<span data-ttu-id="db71d-194">MyVM har stoppats</span><span class="sxs-lookup"><span data-stu-id="db71d-194">MyVM has been stopped</span></span> |
| <span data-ttu-id="db71d-195">Stoppa AzureVMs</span><span class="sxs-lookup"><span data-stu-id="db71d-195">Stop-AzureVMs</span></span> |<span data-ttu-id="db71d-196">Stop-begäran för den virtuella datorn misslyckades</span><span class="sxs-lookup"><span data-stu-id="db71d-196">Stop request for virtual machine failed</span></span> |<span data-ttu-id="db71d-197">MyVM misslyckades toostop</span><span class="sxs-lookup"><span data-stu-id="db71d-197">MyVM failed toostop</span></span> |

<span data-ttu-id="db71d-198">Till exempel försöker hello följande kodavsnitt från en runbook toostart alla virtuella datorer med hello tjänstnamnet *MyServiceName*.</span><span class="sxs-lookup"><span data-stu-id="db71d-198">For example, hello following code snippet from a runbook attempts toostart all virtual machines with hello service name *MyServiceName*.</span></span>  <span data-ttu-id="db71d-199">Om någon av hello startar begäranden misslyckas, kan åtgärder vidtas.</span><span class="sxs-lookup"><span data-stu-id="db71d-199">If any of hello start requests fail, then error actions can be taken.</span></span>

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action tootake in case of success.
        }
        else {
            # Action tootake in case of error.
        }
    }


## <a name="detailed-breakdown"></a><span data-ttu-id="db71d-200">Detaljerad analys</span><span class="sxs-lookup"><span data-stu-id="db71d-200">Detailed breakdown</span></span>
<span data-ttu-id="db71d-201">Nedan följer en detaljerad analys av hello runbooks i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="db71d-201">Following is a detailed breakdown of hello runbooks in this scenario.</span></span>  <span data-ttu-id="db71d-202">Du kan använda den här informationen tooeither anpassa hello runbooks eller bara toolearn från dem för att skapa egna automatiseringsscenarier.</span><span class="sxs-lookup"><span data-stu-id="db71d-202">You can use this information tooeither customize hello runbooks or just toolearn from them for authoring your own automation scenarios.</span></span>

### <a name="parameters"></a><span data-ttu-id="db71d-203">Parametrar</span><span class="sxs-lookup"><span data-stu-id="db71d-203">Parameters</span></span>
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

<span data-ttu-id="db71d-204">hello-arbetsflöde inleds med att hämta hello värden för hello [indataparametrar](#using-the-scenario).</span><span class="sxs-lookup"><span data-stu-id="db71d-204">hello workflow starts by getting hello values for hello [input parameters](#using-the-scenario).</span></span>  <span data-ttu-id="db71d-205">Standardnamn används om hello resursnamn inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="db71d-205">If hello asset names are not provided then default names are used.</span></span>

### <a name="output"></a><span data-ttu-id="db71d-206">Resultat</span><span class="sxs-lookup"><span data-stu-id="db71d-206">Output</span></span>
    # Returns strings with status messages
    [OutputType([String])]

<span data-ttu-id="db71d-207">Den här raden deklarerar att hello utdata från hello runbook ska vara en sträng.</span><span class="sxs-lookup"><span data-stu-id="db71d-207">This line declares that hello output of hello runbook will be a string.</span></span>  <span data-ttu-id="db71d-208">Detta är inte obligatoriskt men det är bästa praxis för när hello runbook används som en [underordnad runbook](automation-child-runbooks.md) så att en överordnad runbook vet hello utdata skriver tooexpect.</span><span class="sxs-lookup"><span data-stu-id="db71d-208">This is not required but is a best practice for when hello runbook is used as a [child runbook](automation-child-runbooks.md) so that a parent runbook will know hello output type tooexpect.</span></span>

### <a name="authentication"></a><span data-ttu-id="db71d-209">Autentisering</span><span class="sxs-lookup"><span data-stu-id="db71d-209">Authentication</span></span>
    # Connect tooAzure and select hello subscription toowork against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

<span data-ttu-id="db71d-210">hello nästa rader ange hello [autentiseringsuppgifter](automation-credentials.md) och Azure-prenumeration som ska användas för hello resten av hello runbook.</span><span class="sxs-lookup"><span data-stu-id="db71d-210">hello next lines set hello [credentials](automation-credentials.md) and Azure subscription that will be used for hello rest of hello runbook.</span></span>
<span data-ttu-id="db71d-211">Första vi använder **Get-AutomationPSCredential** tooget hello tillgång som innehåller hello behörighet med åtkomst toostart och stoppa virtuella datorer i hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="db71d-211">First we use **Get-AutomationPSCredential** tooget hello asset that holds hello credentials with access toostart and stop virtual machines in hello Azure subscription.</span></span> <span data-ttu-id="db71d-212">**Lägg till AzureAccount** använder den här tillgången tooset hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="db71d-212">**Add-AzureAccount** then uses this asset tooset hello credentials.</span></span>  <span data-ttu-id="db71d-213">hello utdata tilldelas tooa dummy variabeln så att den inte ingår i hello runbook-utdata.</span><span class="sxs-lookup"><span data-stu-id="db71d-213">hello output is assigned tooa dummy variable so that it isn't included in hello runbook output.</span></span>  

<span data-ttu-id="db71d-214">Hej variabeltillgång med hello prenumerations-ID: T hämtas med **Get-automationvariable,** och hello prenumeration med **Välj AzureSubscription**.</span><span class="sxs-lookup"><span data-stu-id="db71d-214">hello variable asset with hello subscription ID is then retrieved with **Get-AutomationVariable** and hello subscription set with **Select-AzureSubscription**.</span></span>

### <a name="get-vms"></a><span data-ttu-id="db71d-215">Hämta virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="db71d-215">Get VMs</span></span>
    # If there is a specific cloud service, then get all VMs in hello service,
    # otherwise get all VMs in hello subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

<span data-ttu-id="db71d-216">**Get-AzureVM** är används tooretrieve hello virtuella datorer hello runbook fungerar med.</span><span class="sxs-lookup"><span data-stu-id="db71d-216">**Get-AzureVM** is used tooretrieve hello virtual machines hello runbook will work with.</span></span>  <span data-ttu-id="db71d-217">Om ett värde har angetts i hello **ServiceName** ange variabel och sedan endast hello virtuella datorer med att namn har hämtats.</span><span class="sxs-lookup"><span data-stu-id="db71d-217">If a value is provided in hello **ServiceName** input variable, then only hello virtual machines with that service name are retrieved.</span></span>  <span data-ttu-id="db71d-218">Om **ServiceName** är tom, hämtas alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="db71d-218">If **ServiceName** is empty, then all virtual machines are retrieved.</span></span>

### <a name="startstop-virtual-machines-and-send-output"></a><span data-ttu-id="db71d-219">Starta/stoppa virtuella datorer och skicka utdata</span><span class="sxs-lookup"><span data-stu-id="db71d-219">Start/Stop virtual machines and send output</span></span>
    # Start each of hello stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # hello VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # hello VM needs toobe started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # hello VM failed toostart, so send notice
                Write-Output ($VM.InstanceName + " failed toostart")
            }
            else
            {
                # hello VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

<span data-ttu-id="db71d-220">hello nästa rader gå igenom varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="db71d-220">hello next lines step through each virtual machine.</span></span>  <span data-ttu-id="db71d-221">Först hello **PowerState** av hello virtuella datorn är markerad toosee om den redan är igång eller stoppad beroende hello runbook.</span><span class="sxs-lookup"><span data-stu-id="db71d-221">First hello **PowerState** of hello virtual machine is checked toosee if it is already running or stopped, depending on hello runbook.</span></span>  <span data-ttu-id="db71d-222">Om det redan finns i hello måltillståndet skickas ett meddelande toooutput och hello runbook slutar.</span><span class="sxs-lookup"><span data-stu-id="db71d-222">If it is already in hello target state, then a message is sent toooutput, and hello runbook ends.</span></span>  <span data-ttu-id="db71d-223">Om inte, sedan **Start AzureVM** eller **stoppa AzureVM** är används tooattempt toostart eller stoppa hello virtuell dator med hello resultatet av hello begäran lagrade tooa variabeln.</span><span class="sxs-lookup"><span data-stu-id="db71d-223">If not, then **Start-AzureVM** or **Stop-AzureVM** is used tooattempt toostart or stop hello virtual machine with hello result of hello request stored tooa variable.</span></span>  <span data-ttu-id="db71d-224">Ett meddelande skickas toooutput anger om hello begäran toostart eller stoppa har skickats.</span><span class="sxs-lookup"><span data-stu-id="db71d-224">A message is then sent toooutput specifying whether hello request toostart or stop was submitted successfully.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db71d-225">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db71d-225">Next steps</span></span>
* <span data-ttu-id="db71d-226">toolearn mer information om hur du arbetar med underordnade runbooks finns [underordnade runbooks i Azure Automation](automation-child-runbooks.md)</span><span class="sxs-lookup"><span data-stu-id="db71d-226">toolearn more about working with child runbooks, see [Child runbooks in Azure Automation](automation-child-runbooks.md)</span></span>
* <span data-ttu-id="db71d-227">Mer om toolearn utgående meddelanden under körning av runbook och loggning toohelp felsökning, se [Runbook-utdata och meddelanden i Azure Automation](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="db71d-227">toolearn more about output messages during runbook execution and logging toohelp troubleshoot, see [Runbook output and messages in Azure Automation](automation-runbook-output-and-messages.md)</span></span>

