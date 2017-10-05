---
title: "Azure Automation-resurser i OMS-lösningar | Microsoft Docs"
description: "Lösningar i OMS inkluderar vanligtvis runbooks i Azure Automation för att automatisera processer, till exempel att samla in och bearbetning av övervakningsdata.  Den här artikeln beskriver hur du lägger till runbooks och deras relaterade resurser i en lösning."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 5281462e-f480-4e5e-9c19-022f36dce76d
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c1909183a33ed03d8165671cff25cc8b83b77733
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="adding-azure-automation-resources-to-an-oms-management-solution-preview"></a><span data-ttu-id="2d18a-104">Lägga till Azure Automation-resurser för en OMS-hanteringslösning (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="2d18a-104">Adding Azure Automation resources to an OMS management solution (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="2d18a-105">Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="2d18a-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="2d18a-106">Ett schema som beskrivs nedan kan ändras.</span><span class="sxs-lookup"><span data-stu-id="2d18a-106">Any schema described below is subject to change.</span></span>   


<span data-ttu-id="2d18a-107">[Lösningar för hantering i OMS](operations-management-suite-solutions.md) inkluderar vanligtvis runbooks i Azure Automation för att automatisera processer, till exempel att samla in och bearbetning av övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="2d18a-107">[Management solutions in OMS](operations-management-suite-solutions.md) will typically include runbooks in Azure Automation to automate processes such as collecting and processing monitoring data.</span></span>  <span data-ttu-id="2d18a-108">Förutom runbooks, Automation-konton innehåller resurser, t.ex variabler och scheman som stöd för runbooks som används i lösningen.</span><span class="sxs-lookup"><span data-stu-id="2d18a-108">In addition to runbooks, Automation accounts includes assets such as variables and schedules that support the runbooks used in the solution.</span></span>  <span data-ttu-id="2d18a-109">Den här artikeln beskriver hur du lägger till runbooks och deras relaterade resurser i en lösning.</span><span class="sxs-lookup"><span data-stu-id="2d18a-109">This article describes how to include runbooks and their related resources in a solution.</span></span>

> [!NOTE]
> <span data-ttu-id="2d18a-110">Exemplen i den här artikeln använder parametrar och variabler som är obligatoriska eller vanligt att hanteringslösningar och beskrivs i [och skapa lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="2d18a-110">The samples in this article use parameters and variables that are either required or common to management solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="2d18a-111">Krav</span><span class="sxs-lookup"><span data-stu-id="2d18a-111">Prerequisites</span></span>
<span data-ttu-id="2d18a-112">Den här artikeln förutsätter att du redan är bekant med följande information.</span><span class="sxs-lookup"><span data-stu-id="2d18a-112">This article assumes that you're already familiar with the following information.</span></span>

- <span data-ttu-id="2d18a-113">Så här [skapar en lösning för](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="2d18a-113">How to [create a management solution](operations-management-suite-solutions-creating.md).</span></span>
- <span data-ttu-id="2d18a-114">Struktur för en [lösningsfilen](operations-management-suite-solutions-solution-file.md).</span><span class="sxs-lookup"><span data-stu-id="2d18a-114">The structure of a [solution file](operations-management-suite-solutions-solution-file.md).</span></span>
- <span data-ttu-id="2d18a-115">Så här [skapar Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="2d18a-115">How to [author Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md)</span></span>

## <a name="automation-account"></a><span data-ttu-id="2d18a-116">Automation-konto</span><span class="sxs-lookup"><span data-stu-id="2d18a-116">Automation account</span></span>
<span data-ttu-id="2d18a-117">Alla resurser i Azure Automation finns i en [Automation-konto](../automation/automation-security-overview.md#automation-account-overview).</span><span class="sxs-lookup"><span data-stu-id="2d18a-117">All resources in Azure Automation are contained in an [Automation account](../automation/automation-security-overview.md#automation-account-overview).</span></span>  <span data-ttu-id="2d18a-118">Enligt beskrivningen i [OMS arbetsytan och Automation-konto](operations-management-suite-solutions.md#oms-workspace-and-automation-account) Automation-kontot inte finns med i hanteringslösningen men måste finnas innan lösningen är installerad.</span><span class="sxs-lookup"><span data-stu-id="2d18a-118">As described in [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) the Automation account isn't included in the management solution but must exist before the solution is installed.</span></span>  <span data-ttu-id="2d18a-119">Lösning installationen misslyckas om det inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="2d18a-119">If it isn't available, then the solution install will fail.</span></span>

<span data-ttu-id="2d18a-120">Namnet på varje Automation resursen innehåller namnet på dess Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2d18a-120">The name of each Automation resource includes the name of its Automation account.</span></span>  <span data-ttu-id="2d18a-121">Detta görs i lösningen med den **accountName** parameter som i följande exempel på en runbook-resurs.</span><span class="sxs-lookup"><span data-stu-id="2d18a-121">This is done in the solution with the **accountName** parameter as in the following example of a runbook resource.</span></span>

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a><span data-ttu-id="2d18a-122">Runbooks</span><span class="sxs-lookup"><span data-stu-id="2d18a-122">Runbooks</span></span>
<span data-ttu-id="2d18a-123">Du bör innehålla alla runbooks som används av lösningen i lösningsfilen så att de skapas när lösningen har installerats.</span><span class="sxs-lookup"><span data-stu-id="2d18a-123">You should include any runbooks used by the solution in the solution file so that they're created when the solution is installed.</span></span>  <span data-ttu-id="2d18a-124">Du får inte innehålla innehållet i runbooken i mallen, så du bör publicera en runbook på en allmän plats där den kan nås av alla användare som installerar din lösning.</span><span class="sxs-lookup"><span data-stu-id="2d18a-124">You cannot contain the body of the runbook in the template though, so you should publish the runbook to a public location where it can be accessed by any user installing your solution.</span></span>

<span data-ttu-id="2d18a-125">[Azure Automation-runbook](../automation/automation-runbook-types.md) resurser har en typ av **Microsoft.Automation/automationAccounts/runbooks** och har följande struktur.</span><span class="sxs-lookup"><span data-stu-id="2d18a-125">[Azure Automation runbook](../automation/automation-runbook-types.md) resources have a type of **Microsoft.Automation/automationAccounts/runbooks** and have the following structure.</span></span> <span data-ttu-id="2d18a-126">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra parameternamn.</span><span class="sxs-lookup"><span data-stu-id="2d18a-126">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

    {
        "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
        "type": "Microsoft.Automation/automationAccounts/runbooks",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "location": "[parameters('regionId')]",
        "tags": { },
        "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
                "uri": "[variables('Runbook').Uri]",
                "version": [variables('Runbook').Version]"
            }
        }
    }


<span data-ttu-id="2d18a-127">I följande tabell beskrivs egenskaperna för runbooks.</span><span class="sxs-lookup"><span data-stu-id="2d18a-127">The properties for runbooks are described in the following table.</span></span>

| <span data-ttu-id="2d18a-128">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2d18a-128">Property</span></span> | <span data-ttu-id="2d18a-129">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-129">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2d18a-130">runbookType</span><span class="sxs-lookup"><span data-stu-id="2d18a-130">runbookType</span></span> |<span data-ttu-id="2d18a-131">Anger vilka typer av runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-131">Specifies the types of the runbook.</span></span> <br><br> <span data-ttu-id="2d18a-132">Skript - PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="2d18a-132">Script - PowerShell script</span></span> <br><span data-ttu-id="2d18a-133">PowerShell - PowerShell-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="2d18a-133">PowerShell - PowerShell workflow</span></span> <br> <span data-ttu-id="2d18a-134">GraphPowerShell - grafisk PowerShell-skriptet runbook</span><span class="sxs-lookup"><span data-stu-id="2d18a-134">GraphPowerShell - Graphical PowerShell script runbook</span></span> <br> <span data-ttu-id="2d18a-135">GraphPowerShellWorkflow - grafisk PowerShell-arbetsflödesrunbook</span><span class="sxs-lookup"><span data-stu-id="2d18a-135">GraphPowerShellWorkflow - Graphical PowerShell workflow runbook</span></span> |
| <span data-ttu-id="2d18a-136">logProgress</span><span class="sxs-lookup"><span data-stu-id="2d18a-136">logProgress</span></span> |<span data-ttu-id="2d18a-137">Anger om [vidare poster](../automation/automation-runbook-output-and-messages.md) ska genereras för runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-137">Specifies whether [progress records](../automation/automation-runbook-output-and-messages.md) should be generated for the runbook.</span></span> |
| <span data-ttu-id="2d18a-138">logVerbose</span><span class="sxs-lookup"><span data-stu-id="2d18a-138">logVerbose</span></span> |<span data-ttu-id="2d18a-139">Anger om [utförliga poster](../automation/automation-runbook-output-and-messages.md) ska genereras för runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-139">Specifies whether [verbose records](../automation/automation-runbook-output-and-messages.md) should be generated for the runbook.</span></span> |
| <span data-ttu-id="2d18a-140">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-140">description</span></span> |<span data-ttu-id="2d18a-141">Valfri beskrivning för runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-141">Optional description for the runbook.</span></span> |
| <span data-ttu-id="2d18a-142">publishContentLink</span><span class="sxs-lookup"><span data-stu-id="2d18a-142">publishContentLink</span></span> |<span data-ttu-id="2d18a-143">Anger innehållet i runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-143">Specifies the content of the runbook.</span></span> <br><br><span data-ttu-id="2d18a-144">URI - Uri för att innehållet i runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-144">uri - Uri to the content of the runbook.</span></span>  <span data-ttu-id="2d18a-145">Det här är en .ps1-fil för PowerShell-skript och runbooks och en exporterade grafiska runbook-fil för en grafisk runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-145">This will be a .ps1 file for PowerShell and Script runbooks, and an exported graphical runbook file for a Graph runbook.</span></span>  <br> <span data-ttu-id="2d18a-146">version - versionen av runbook för egna spårning.</span><span class="sxs-lookup"><span data-stu-id="2d18a-146">version - Version of the runbook for your own tracking.</span></span> |


## <a name="automation-jobs"></a><span data-ttu-id="2d18a-147">Automation-jobb</span><span class="sxs-lookup"><span data-stu-id="2d18a-147">Automation jobs</span></span>
<span data-ttu-id="2d18a-148">När du startar en runbook i Azure Automation skapas ett automation-jobb.</span><span class="sxs-lookup"><span data-stu-id="2d18a-148">When you start a runbook in Azure Automation, it creates an automation job.</span></span>  <span data-ttu-id="2d18a-149">Du kan lägga till en resurs för automation-jobb din lösning för att automatiskt starta en runbook när hanteringslösningen som är installerad.</span><span class="sxs-lookup"><span data-stu-id="2d18a-149">You can add an automation job resource to your solution to automatically start a runbook when the management solution is installed.</span></span>  <span data-ttu-id="2d18a-150">Den här metoden används vanligtvis för att starta runbooks som används för inledande konfiguration av lösningen.</span><span class="sxs-lookup"><span data-stu-id="2d18a-150">This method is typically used to start runbooks that are used for initial configuration of the solution.</span></span>  <span data-ttu-id="2d18a-151">Om du vill starta en runbook med jämna mellanrum, skapa en [schema](#schedules) och en [Jobbschema](#job-schedules)</span><span class="sxs-lookup"><span data-stu-id="2d18a-151">To start a runbook at regular intervals, create a [schedule](#schedules) and a [job schedule](#job-schedules)</span></span>

<span data-ttu-id="2d18a-152">Resurser för jobbet har en typ av **Microsoft.Automation/automationAccounts/jobs** och har följande struktur.</span><span class="sxs-lookup"><span data-stu-id="2d18a-152">Job resources have a type of **Microsoft.Automation/automationAccounts/jobs** and have the following structure.</span></span>  <span data-ttu-id="2d18a-153">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra parameternamn.</span><span class="sxs-lookup"><span data-stu-id="2d18a-153">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', parameters('Runbook').JobGuid)]",
      "type": "Microsoft.Automation/automationAccounts/jobs",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
      ],
      "tags": { },
      "properties": {
        "runbook": {
          "name": "[variables('Runbook').Name]"
        },
        "parameters": {
          "Parameter1": "[[variables('Runbook').Parameter1]",
          "Parameter2": "[[variables('Runbook').Parameter2]"
        }
      }
    }

<span data-ttu-id="2d18a-154">I följande tabell beskrivs egenskaperna för automation-jobb.</span><span class="sxs-lookup"><span data-stu-id="2d18a-154">The properties for automation jobs are described in the following table.</span></span>

| <span data-ttu-id="2d18a-155">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2d18a-155">Property</span></span> | <span data-ttu-id="2d18a-156">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-156">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2d18a-157">runbook</span><span class="sxs-lookup"><span data-stu-id="2d18a-157">runbook</span></span> |<span data-ttu-id="2d18a-158">Namn på enkel enhet med namnet på runbook att starta.</span><span class="sxs-lookup"><span data-stu-id="2d18a-158">Single name entity with the name of the runbook to start.</span></span> |
| <span data-ttu-id="2d18a-159">Parametrar</span><span class="sxs-lookup"><span data-stu-id="2d18a-159">parameters</span></span> |<span data-ttu-id="2d18a-160">Entitet för varje parametervärde som krävs av runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-160">Entity for each parameter value required by the runbook.</span></span> |

<span data-ttu-id="2d18a-161">Jobbet innehåller runbook-namn och ett parametervärde som skickas till runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-161">The job includes the runbook name and any parameter values to be sent to the runbook.</span></span>  <span data-ttu-id="2d18a-162">Jobbet ska [beror på](operations-management-suite-solutions-solution-file.md#resources) runbook startar sedan runbook måste skapas innan jobbet.</span><span class="sxs-lookup"><span data-stu-id="2d18a-162">The job should [depend on](operations-management-suite-solutions-solution-file.md#resources) the runbook that it's starting since the runbook must be created before the job.</span></span>  <span data-ttu-id="2d18a-163">Om du har flera runbooks som ska startas kan du definiera deras inbördes ordning genom att ett jobb är beroende av andra jobb som ska köras första.</span><span class="sxs-lookup"><span data-stu-id="2d18a-163">If you have multiple runbooks that should be started you can define their order by having a job depend on any other jobs that should be run first.</span></span>

<span data-ttu-id="2d18a-164">Namnet på en resurs för jobbet måste innehålla en GUID som tilldelas vanligtvis av en parameter.</span><span class="sxs-lookup"><span data-stu-id="2d18a-164">The name of a job resource must contain a GUID which is typically assigned by a parameter.</span></span>  <span data-ttu-id="2d18a-165">Du kan läsa mer om GUID-parametrar i [och skapa lösningar i Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="2d18a-165">You can read more about GUID parameters in [Creating solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).</span></span>  


## <a name="certificates"></a><span data-ttu-id="2d18a-166">Certifikat</span><span class="sxs-lookup"><span data-stu-id="2d18a-166">Certificates</span></span>
<span data-ttu-id="2d18a-167">[Azure Automation-certifikat](../automation/automation-certificates.md) har en typ av **Microsoft.Automation/automationAccounts/certificates** och har följande struktur.</span><span class="sxs-lookup"><span data-stu-id="2d18a-167">[Azure Automation certificates](../automation/automation-certificates.md) have a type of **Microsoft.Automation/automationAccounts/certificates** and have the following structure.</span></span> <span data-ttu-id="2d18a-168">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra parameternamn.</span><span class="sxs-lookup"><span data-stu-id="2d18a-168">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
      "type": "Microsoft.Automation/automationAccounts/certificates",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "base64Value": "[variables('Certificate').Base64Value]",
        "thumbprint": "[variables('Certificate').Thumbprint]"
      }
    }



<span data-ttu-id="2d18a-169">I följande tabell beskrivs egenskaperna för certifikat resurser.</span><span class="sxs-lookup"><span data-stu-id="2d18a-169">The properties for Certificates resources are described in the following table.</span></span>

| <span data-ttu-id="2d18a-170">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2d18a-170">Property</span></span> | <span data-ttu-id="2d18a-171">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-171">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2d18a-172">base64Value</span><span class="sxs-lookup"><span data-stu-id="2d18a-172">base64Value</span></span> |<span data-ttu-id="2d18a-173">Base 64-värde för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="2d18a-173">Base 64 value for the certificate.</span></span> |
| <span data-ttu-id="2d18a-174">tumavtrycket</span><span class="sxs-lookup"><span data-stu-id="2d18a-174">thumbprint</span></span> |<span data-ttu-id="2d18a-175">Tumavtryck för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="2d18a-175">Thumbprint for the certificate.</span></span> |



## <a name="credentials"></a><span data-ttu-id="2d18a-176">Autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="2d18a-176">Credentials</span></span>
<span data-ttu-id="2d18a-177">[Autentiseringsuppgifter för Azure Automation](../automation/automation-credentials.md) har en typ av **Microsoft.Automation/automationAccounts/credentials** och har följande struktur.</span><span class="sxs-lookup"><span data-stu-id="2d18a-177">[Azure Automation credentials](../automation/automation-credentials.md) have a type of **Microsoft.Automation/automationAccounts/credentials** and have the following structure.</span></span>  <span data-ttu-id="2d18a-178">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra parameternamn.</span><span class="sxs-lookup"><span data-stu-id="2d18a-178">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 


    {
      "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
      "type": "Microsoft.Automation/automationAccounts/credentials",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "userName": "[parameters('credentialUsername')]",
        "password": "[parameters('credentialPassword')]"
      }
    }

<span data-ttu-id="2d18a-179">I följande tabell beskrivs egenskaperna för autentiseringsuppgifter resurser.</span><span class="sxs-lookup"><span data-stu-id="2d18a-179">The properties for Credential resources are described in the following table.</span></span>

| <span data-ttu-id="2d18a-180">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2d18a-180">Property</span></span> | <span data-ttu-id="2d18a-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-181">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2d18a-182">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="2d18a-182">userName</span></span> |<span data-ttu-id="2d18a-183">Användarnamn för autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2d18a-183">User name for the credential.</span></span> |
| <span data-ttu-id="2d18a-184">lösenord</span><span class="sxs-lookup"><span data-stu-id="2d18a-184">password</span></span> |<span data-ttu-id="2d18a-185">Lösenordet för autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="2d18a-185">Password for the credential.</span></span> |


## <a name="schedules"></a><span data-ttu-id="2d18a-186">Scheman</span><span class="sxs-lookup"><span data-stu-id="2d18a-186">Schedules</span></span>
<span data-ttu-id="2d18a-187">[Azure Automation-scheman](../automation/automation-schedules.md) har en typ av **Microsoft.Automation/automationAccounts/schedules** och har de följande struktur.</span><span class="sxs-lookup"><span data-stu-id="2d18a-187">[Azure Automation schedules](../automation/automation-schedules.md) have a type of **Microsoft.Automation/automationAccounts/schedules** and have the the following structure.</span></span> <span data-ttu-id="2d18a-188">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra parameternamn.</span><span class="sxs-lookup"><span data-stu-id="2d18a-188">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
      "type": "microsoft.automation/automationAccounts/schedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Schedule').Description]",
        "startTime": "[parameters('scheduleStartTime')]",
        "timeZone": "[parameters('scheduleTimeZone')]",
        "isEnabled": "[variables('Schedule').IsEnabled]",
        "interval": "[variables('Schedule').Interval]",
        "frequency": "[variables('Schedule').Frequency]"
      }
    }

<span data-ttu-id="2d18a-189">I följande tabell beskrivs egenskaperna för schemalägga resurser.</span><span class="sxs-lookup"><span data-stu-id="2d18a-189">The properties for schedule resources are described in the following table.</span></span>

| <span data-ttu-id="2d18a-190">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2d18a-190">Property</span></span> | <span data-ttu-id="2d18a-191">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-191">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2d18a-192">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-192">description</span></span> |<span data-ttu-id="2d18a-193">Valfri beskrivning för schemat.</span><span class="sxs-lookup"><span data-stu-id="2d18a-193">Optional description for the schedule.</span></span> |
| <span data-ttu-id="2d18a-194">startTime</span><span class="sxs-lookup"><span data-stu-id="2d18a-194">startTime</span></span> |<span data-ttu-id="2d18a-195">Anger starttiden för ett schema som ett DateTime-objekt.</span><span class="sxs-lookup"><span data-stu-id="2d18a-195">Specifies the start time of a schedule as a DateTime object.</span></span> <span data-ttu-id="2d18a-196">En sträng kan tillhandahållas om det kan konverteras till en giltig DateTime.</span><span class="sxs-lookup"><span data-stu-id="2d18a-196">A string can be provided if it can be converted to a valid DateTime.</span></span> |
| <span data-ttu-id="2d18a-197">IsEnabled</span><span class="sxs-lookup"><span data-stu-id="2d18a-197">isEnabled</span></span> |<span data-ttu-id="2d18a-198">Anger om schemat har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="2d18a-198">Specifies whether the schedule is enabled.</span></span> |
| <span data-ttu-id="2d18a-199">intervall</span><span class="sxs-lookup"><span data-stu-id="2d18a-199">interval</span></span> |<span data-ttu-id="2d18a-200">Typ av intervall för schemat.</span><span class="sxs-lookup"><span data-stu-id="2d18a-200">The type of interval for the schedule.</span></span><br><br><span data-ttu-id="2d18a-201">dag</span><span class="sxs-lookup"><span data-stu-id="2d18a-201">day</span></span><br><span data-ttu-id="2d18a-202">timme</span><span class="sxs-lookup"><span data-stu-id="2d18a-202">hour</span></span> |
| <span data-ttu-id="2d18a-203">frekvens</span><span class="sxs-lookup"><span data-stu-id="2d18a-203">frequency</span></span> |<span data-ttu-id="2d18a-204">Frekvensen som schemat ska utlösas i antal dagar eller timmar.</span><span class="sxs-lookup"><span data-stu-id="2d18a-204">Frequency that the schedule should fire in number of days or hours.</span></span> |

<span data-ttu-id="2d18a-205">Måste ha en starttid med ett större värde än den aktuella tiden.</span><span class="sxs-lookup"><span data-stu-id="2d18a-205">Schedules must have a start time with a value greater than the current time.</span></span>  <span data-ttu-id="2d18a-206">Du kan inte ange det här värdet till en variabel eftersom du har ingen möjlighet att veta när den ska installeras.</span><span class="sxs-lookup"><span data-stu-id="2d18a-206">You cannot provide this value with a variable since you would have no way of knowing when it's going to be installed.</span></span>

<span data-ttu-id="2d18a-207">Använd någon av följande två strategier när du använder schemalägga resurser i en lösning.</span><span class="sxs-lookup"><span data-stu-id="2d18a-207">Use one of the following two strategies when using schedule resources in a solution.</span></span>

- <span data-ttu-id="2d18a-208">Använda en parameter för starttiden för schemat.</span><span class="sxs-lookup"><span data-stu-id="2d18a-208">Use a parameter for the start time of the schedule.</span></span>  <span data-ttu-id="2d18a-209">Detta uppmanar användaren att ange ett värde när de installerar lösningen.</span><span class="sxs-lookup"><span data-stu-id="2d18a-209">This will prompt the user to provide a value when they install the solution.</span></span>  <span data-ttu-id="2d18a-210">Om du har flera scheman, kan du använda en enda parameter-värdet för fler än ett.</span><span class="sxs-lookup"><span data-stu-id="2d18a-210">If you have multiple schedules, you could use a single parameter value for more than one of them.</span></span>
- <span data-ttu-id="2d18a-211">Skapa schemat med en runbook som startar när lösningen har installerats.</span><span class="sxs-lookup"><span data-stu-id="2d18a-211">Create the schedules using a runbook that starts when the solution is installed.</span></span>  <span data-ttu-id="2d18a-212">Detta tar bort behovet av användaren som anger en tid, men du kan inte innehålla schemat i din lösning så tas den bort när lösningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="2d18a-212">This removes the requirement of the user to specify a time, but you can't contain the schedule in your solution so it will be removed when the solution is removed.</span></span>


### <a name="job-schedules"></a><span data-ttu-id="2d18a-213">Jobbscheman</span><span class="sxs-lookup"><span data-stu-id="2d18a-213">Job schedules</span></span>
<span data-ttu-id="2d18a-214">Jobbet schemalägga resurser länkar en runbook med ett schema.</span><span class="sxs-lookup"><span data-stu-id="2d18a-214">Job schedule resources link a runbook with a schedule.</span></span>  <span data-ttu-id="2d18a-215">De har en typ av **Microsoft.Automation/automationAccounts/jobSchedules** och har de följande struktur.</span><span class="sxs-lookup"><span data-stu-id="2d18a-215">They have a type of **Microsoft.Automation/automationAccounts/jobSchedules** and have the the following structure.</span></span>  <span data-ttu-id="2d18a-216">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra parameternamn.</span><span class="sxs-lookup"><span data-stu-id="2d18a-216">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
      "type": "microsoft.automation/automationAccounts/jobSchedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
        "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
      ],
      "tags": {
      },
      "properties": {
        "schedule": {
          "name": "[variables('Schedule').Name]"
        },
        "runbook": {
          "name": "[variables('Runbook').Name]"
        }
      }
    }


<span data-ttu-id="2d18a-217">I följande tabell beskrivs egenskaperna för jobbscheman.</span><span class="sxs-lookup"><span data-stu-id="2d18a-217">The properties for job schedules are described in the following table.</span></span>

| <span data-ttu-id="2d18a-218">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2d18a-218">Property</span></span> | <span data-ttu-id="2d18a-219">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-219">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2d18a-220">namn på schema</span><span class="sxs-lookup"><span data-stu-id="2d18a-220">schedule name</span></span> |<span data-ttu-id="2d18a-221">Enskild **namn** entitet med namnet på schemat.</span><span class="sxs-lookup"><span data-stu-id="2d18a-221">Single **name** entity with the name of the schedule.</span></span> |
| <span data-ttu-id="2d18a-222">Runbook-namn</span><span class="sxs-lookup"><span data-stu-id="2d18a-222">runbook name</span></span>  |<span data-ttu-id="2d18a-223">Enskild **namn** entitet med namnet på runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-223">Single **name** entity with the name of the runbook.</span></span>  |



## <a name="variables"></a><span data-ttu-id="2d18a-224">Variabler</span><span class="sxs-lookup"><span data-stu-id="2d18a-224">Variables</span></span>
<span data-ttu-id="2d18a-225">[Azure Automation-variabler](../automation/automation-variables.md) har en typ av **Microsoft.Automation/automationAccounts/variables** och har följande struktur.</span><span class="sxs-lookup"><span data-stu-id="2d18a-225">[Azure Automation variables](../automation/automation-variables.md) have a type of **Microsoft.Automation/automationAccounts/variables** and have the following structure.</span></span>  <span data-ttu-id="2d18a-226">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra parameternamn.</span><span class="sxs-lookup"><span data-stu-id="2d18a-226">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span>

    {
      "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
      "type": "microsoft.automation/automationAccounts/variables",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Variable').Description]",
        "isEncrypted": "[variables('Variable').Encrypted]",
        "type": "[variables('Variable').Type]",
        "value": "[variables('Variable').Value]"
      }
    }

<span data-ttu-id="2d18a-227">I följande tabell beskrivs egenskaperna för variabeln resurser.</span><span class="sxs-lookup"><span data-stu-id="2d18a-227">The properties for variable resources are described in the following table.</span></span>

| <span data-ttu-id="2d18a-228">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2d18a-228">Property</span></span> | <span data-ttu-id="2d18a-229">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-229">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2d18a-230">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-230">description</span></span> | <span data-ttu-id="2d18a-231">Valfri beskrivning för variabeln.</span><span class="sxs-lookup"><span data-stu-id="2d18a-231">Optional description for the variable.</span></span> |
| <span data-ttu-id="2d18a-232">isEncrypted</span><span class="sxs-lookup"><span data-stu-id="2d18a-232">isEncrypted</span></span> | <span data-ttu-id="2d18a-233">Anger om variabeln ska krypteras.</span><span class="sxs-lookup"><span data-stu-id="2d18a-233">Specifies whether the variable should be encrypted.</span></span> |
| <span data-ttu-id="2d18a-234">typ</span><span class="sxs-lookup"><span data-stu-id="2d18a-234">type</span></span> | <span data-ttu-id="2d18a-235">Den här egenskapen har för närvarande ingen effekt.</span><span class="sxs-lookup"><span data-stu-id="2d18a-235">This property currently has no effect.</span></span>  <span data-ttu-id="2d18a-236">Datatypen för variabeln bestäms av det ursprungliga värdet.</span><span class="sxs-lookup"><span data-stu-id="2d18a-236">The data type of the variable will be determined by the initial value.</span></span> |
| <span data-ttu-id="2d18a-237">värde</span><span class="sxs-lookup"><span data-stu-id="2d18a-237">value</span></span> | <span data-ttu-id="2d18a-238">Värdet för variabeln.</span><span class="sxs-lookup"><span data-stu-id="2d18a-238">Value for the variable.</span></span> |

> [!NOTE]
> <span data-ttu-id="2d18a-239">Den **typen** egenskapen för närvarande har ingen effekt på variabeln som skapas.</span><span class="sxs-lookup"><span data-stu-id="2d18a-239">The **type** property currently has no effect on the variable being created.</span></span>  <span data-ttu-id="2d18a-240">Datatypen för variabeln bestäms av värdet.</span><span class="sxs-lookup"><span data-stu-id="2d18a-240">The data type for the variable will be determined by the value.</span></span>  

<span data-ttu-id="2d18a-241">Om du anger det initiala värdet för variabeln, måste den konfigureras som rätt datatyp.</span><span class="sxs-lookup"><span data-stu-id="2d18a-241">If you set the initial value for the variable, it must be configured as the correct data type.</span></span>  <span data-ttu-id="2d18a-242">Följande tabell innehåller olika datatyper som är tillåtna och deras syntax.</span><span class="sxs-lookup"><span data-stu-id="2d18a-242">The following table provides the different data types allowable and their syntax.</span></span>  <span data-ttu-id="2d18a-243">Observera att värdena i JSON förväntas alltid stå inom citattecken med några specialtecken inom citattecken.</span><span class="sxs-lookup"><span data-stu-id="2d18a-243">Note that values in JSON are expected to always be enclosed in quotes with any special characters within the quotes.</span></span>  <span data-ttu-id="2d18a-244">Till exempel ett strängvärde skulle anges med citattecken runt strängen (med hjälp av escape-tecken (\\)) när ett numeriskt värde skulle anges med en uppsättning av citattecken.</span><span class="sxs-lookup"><span data-stu-id="2d18a-244">For example, a string value would be specified by quotes around the string (using the escape character (\\)) while a numeric value would be specified with one set of quotes.</span></span>

| <span data-ttu-id="2d18a-245">Datatyp</span><span class="sxs-lookup"><span data-stu-id="2d18a-245">Data type</span></span> | <span data-ttu-id="2d18a-246">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-246">Description</span></span> | <span data-ttu-id="2d18a-247">Exempel</span><span class="sxs-lookup"><span data-stu-id="2d18a-247">Example</span></span> | <span data-ttu-id="2d18a-248">Matchar</span><span class="sxs-lookup"><span data-stu-id="2d18a-248">Resolves to</span></span> |
|:--|:--|:--|:--|
| <span data-ttu-id="2d18a-249">Sträng</span><span class="sxs-lookup"><span data-stu-id="2d18a-249">string</span></span>   | <span data-ttu-id="2d18a-250">Värdet omges av dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="2d18a-250">Enclose value in double quotes.</span></span>  | <span data-ttu-id="2d18a-251">”\"Hello world\"”</span><span class="sxs-lookup"><span data-stu-id="2d18a-251">"\"Hello world\""</span></span> | <span data-ttu-id="2d18a-252">”Hello world”</span><span class="sxs-lookup"><span data-stu-id="2d18a-252">"Hello world"</span></span> |
| <span data-ttu-id="2d18a-253">numeriskt</span><span class="sxs-lookup"><span data-stu-id="2d18a-253">numeric</span></span>  | <span data-ttu-id="2d18a-254">Numeriskt värde med enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="2d18a-254">Numeric value with single quotes.</span></span>| <span data-ttu-id="2d18a-255">"64"</span><span class="sxs-lookup"><span data-stu-id="2d18a-255">"64"</span></span> | <span data-ttu-id="2d18a-256">64</span><span class="sxs-lookup"><span data-stu-id="2d18a-256">64</span></span> |
| <span data-ttu-id="2d18a-257">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="2d18a-257">boolean</span></span>  | <span data-ttu-id="2d18a-258">**SANT** eller **FALSKT** inom citattecken.</span><span class="sxs-lookup"><span data-stu-id="2d18a-258">**true** or **false** in quotes.</span></span>  <span data-ttu-id="2d18a-259">Observera att det här värdet måste vara gemener.</span><span class="sxs-lookup"><span data-stu-id="2d18a-259">Note that this value must be lowercase.</span></span> | <span data-ttu-id="2d18a-260">”true”</span><span class="sxs-lookup"><span data-stu-id="2d18a-260">"true"</span></span> | <span data-ttu-id="2d18a-261">SANT</span><span class="sxs-lookup"><span data-stu-id="2d18a-261">true</span></span> |
| <span data-ttu-id="2d18a-262">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="2d18a-262">datetime</span></span> | <span data-ttu-id="2d18a-263">Serialiserad date-värde.</span><span class="sxs-lookup"><span data-stu-id="2d18a-263">Serialized date value.</span></span><br><span data-ttu-id="2d18a-264">Du kan använda cmdleten ConvertTo Json i PowerShell för att generera det här värdet för ett visst datum.</span><span class="sxs-lookup"><span data-stu-id="2d18a-264">You can use the ConvertTo-Json cmdlet in PowerShell to generate this value for a particular date.</span></span><br><span data-ttu-id="2d18a-265">Exempel: get-date ”2017-5/24 13:14:57” \\</span><span class="sxs-lookup"><span data-stu-id="2d18a-265">Example: get-date "5/24/2017 13:14:57" \\</span></span>| <span data-ttu-id="2d18a-266">ConvertTo Json</span><span class="sxs-lookup"><span data-stu-id="2d18a-266">ConvertTo-Json</span></span> | <span data-ttu-id="2d18a-267">”\\/Date(1495656897378)\\/”</span><span class="sxs-lookup"><span data-stu-id="2d18a-267">"\\/Date(1495656897378)\\/"</span></span> | <span data-ttu-id="2d18a-268">2017-05-24 13:14:57</span><span class="sxs-lookup"><span data-stu-id="2d18a-268">2017-05-24 13:14:57</span></span> |

## <a name="modules"></a><span data-ttu-id="2d18a-269">Moduler</span><span class="sxs-lookup"><span data-stu-id="2d18a-269">Modules</span></span>
<span data-ttu-id="2d18a-270">Management-lösningen behöver inte definiera [global modul](../automation/automation-integration-modules.md) används av dina runbooks eftersom de alltid är tillgängliga i ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2d18a-270">Your management solution does not need to define [global modules](../automation/automation-integration-modules.md) used by your runbooks because they will always be available in your Automation account.</span></span>  <span data-ttu-id="2d18a-271">Du behöver innehåller en resurs för alla moduler som används av dina runbooks.</span><span class="sxs-lookup"><span data-stu-id="2d18a-271">You do need to include a resource for any other module used by your runbooks.</span></span>

<span data-ttu-id="2d18a-272">[Integreringsmoduler](../automation/automation-integration-modules.md) har en typ av **Microsoft.Automation/automationAccounts/modules** och har följande struktur.</span><span class="sxs-lookup"><span data-stu-id="2d18a-272">[Integration modules](../automation/automation-integration-modules.md) have a type of **Microsoft.Automation/automationAccounts/modules** and have the following structure.</span></span>  <span data-ttu-id="2d18a-273">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra parameternamn.</span><span class="sxs-lookup"><span data-stu-id="2d18a-273">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span>

    {
      "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
      "type": "Microsoft.Automation/automationAccounts/modules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "dependsOn": [
      ],
      "properties": {
        "contentLink": {
          "uri": "[variables('Module').Uri]"
        }
      }
    }


<span data-ttu-id="2d18a-274">I följande tabell beskrivs egenskaperna för modulen resurser.</span><span class="sxs-lookup"><span data-stu-id="2d18a-274">The properties for module resources are described in the following table.</span></span>

| <span data-ttu-id="2d18a-275">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2d18a-275">Property</span></span> | <span data-ttu-id="2d18a-276">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d18a-276">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="2d18a-277">contentLink</span><span class="sxs-lookup"><span data-stu-id="2d18a-277">contentLink</span></span> |<span data-ttu-id="2d18a-278">Anger innehållet i modulen.</span><span class="sxs-lookup"><span data-stu-id="2d18a-278">Specifies the content of the module.</span></span> <br><br><span data-ttu-id="2d18a-279">URI - Uri för att innehållet i modulen.</span><span class="sxs-lookup"><span data-stu-id="2d18a-279">uri - Uri to the content of the module.</span></span>  <span data-ttu-id="2d18a-280">Det här är en .ps1-fil för PowerShell-skript och runbooks och en exporterade grafiska runbook-fil för en grafisk runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-280">This will be a .ps1 file for PowerShell and Script runbooks, and an exported graphical runbook file for a Graph runbook.</span></span>  <br> <span data-ttu-id="2d18a-281">version - versionen av modulen för egna spårning.</span><span class="sxs-lookup"><span data-stu-id="2d18a-281">version - Version of the module for your own tracking.</span></span> |

<span data-ttu-id="2d18a-282">Runbook ska beror på resursen modulen så att den har skapats innan runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-282">The runbook should depend on the module resource to ensure that it's created before the runbook.</span></span>

### <a name="updating-modules"></a><span data-ttu-id="2d18a-283">Uppdatera moduler</span><span class="sxs-lookup"><span data-stu-id="2d18a-283">Updating modules</span></span>
<span data-ttu-id="2d18a-284">Om du uppdaterar en lösning som innehåller en runbook som använder ett schema, och den nya versionen av din lösning har en ny modul som används av denna runbook, kan runbook använda den gamla versionen av modulen.</span><span class="sxs-lookup"><span data-stu-id="2d18a-284">If you update a management solution that includes a runbook that uses a schedule, and the new version of your solution has a new module used by that runbook, then the runbook may use the old version of the module.</span></span>  <span data-ttu-id="2d18a-285">Du bör inkludera följande runbooks i din lösning och skapar ett jobb för att köra dem före andra runbooks.</span><span class="sxs-lookup"><span data-stu-id="2d18a-285">You should include the following runbooks in your solution and create a job to run them before any other runbooks.</span></span>  <span data-ttu-id="2d18a-286">Detta säkerställer att alla moduler som är uppdaterade som krävs innan runbooks har lästs in.</span><span class="sxs-lookup"><span data-stu-id="2d18a-286">This will ensure that any modules are updated as required before the runbooks are loaded.</span></span>

* <span data-ttu-id="2d18a-287">[Uppdatera ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) säkerställer att alla moduler som används av runbooks i din lösning är den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="2d18a-287">[Update-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) will ensure that all of the modules used by runbooks in your solution are the latest version.</span></span>  
* <span data-ttu-id="2d18a-288">[ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) ska registrera om alla resurser som schemat så att runbooks kopplade till dem med använder de senaste modulerna.</span><span class="sxs-lookup"><span data-stu-id="2d18a-288">[ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) will reregister all of the schedule resources to ensure that the runbooks linked to them with use the latest modules.</span></span>




## <a name="sample"></a><span data-ttu-id="2d18a-289">Exempel</span><span class="sxs-lookup"><span data-stu-id="2d18a-289">Sample</span></span>
<span data-ttu-id="2d18a-290">Följande är ett exempel på en lösning som omfattar som innehåller följande resurser:</span><span class="sxs-lookup"><span data-stu-id="2d18a-290">Following is a sample of a solution that include that includes the following resources:</span></span>

- <span data-ttu-id="2d18a-291">Runbook.</span><span class="sxs-lookup"><span data-stu-id="2d18a-291">Runbook.</span></span>  <span data-ttu-id="2d18a-292">Det här är en exempel-runbook som lagras i en offentlig GitHub-databas.</span><span class="sxs-lookup"><span data-stu-id="2d18a-292">This is a sample runbook stored in a public GitHub repository.</span></span>
- <span data-ttu-id="2d18a-293">Automation-jobb som startar runbook när lösningen har installerats.</span><span class="sxs-lookup"><span data-stu-id="2d18a-293">Automation job that starts the runbook when the solution is installed.</span></span>
- <span data-ttu-id="2d18a-294">Schemat och schema för att starta runbook med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="2d18a-294">Schedule and job schedule to start the runbook at regular intervals.</span></span>
- <span data-ttu-id="2d18a-295">Certifikat.</span><span class="sxs-lookup"><span data-stu-id="2d18a-295">Certificate.</span></span>
- <span data-ttu-id="2d18a-296">Autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2d18a-296">Credential.</span></span>
- <span data-ttu-id="2d18a-297">Variabel.</span><span class="sxs-lookup"><span data-stu-id="2d18a-297">Variable.</span></span>
- <span data-ttu-id="2d18a-298">Modul.</span><span class="sxs-lookup"><span data-stu-id="2d18a-298">Module.</span></span>  <span data-ttu-id="2d18a-299">Det här är den [OMSIngestionAPI modulen](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) för att skriva data till logganalys.</span><span class="sxs-lookup"><span data-stu-id="2d18a-299">This is the [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) for writing data to Log Analytics.</span></span> 

<span data-ttu-id="2d18a-300">Används [standardlösningen parametrar](operations-management-suite-solutions-solution-file.md#parameters) variabler som ofta används i en lösning i stället för hardcoding värden i resursdefinitionerna.</span><span class="sxs-lookup"><span data-stu-id="2d18a-300">The sample uses [standard solution parameters](operations-management-suite-solutions-solution-file.md#parameters) variables that would commonly be used in a solution as opposed to hardcoding values in the resource definitions.</span></span>


    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Log Analytics workspace."
          }
        },
        "accountName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Automation account."
          }
        },
        "workspaceregionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Log Analytics workspace."
          }
        },
        "regionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Automation account."
          }
        },
        "pricingTier": {
          "type": "string",
          "metadata": {
            "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account."
          }
        },
        "certificateBase64Value": {
          "type": "string",
          "metadata": {
            "Description": "Base 64 value for certificate."
          }
        },
        "certificateThumbprint": {
          "type": "securestring",
          "metadata": {
            "Description": "Thumbprint for certificate."
          }
        },
        "credentialUsername": {
          "type": "string",
          "metadata": {
            "Description": "Username for credential."
          }
        },
        "credentialPassword": {
          "type": "securestring",
          "metadata": {
            "Description": "Password for credential."
          }
        },
        "scheduleStartTime": {
          "type": "string",
          "metadata": {
            "Description": "Start time for shedule."
          }
        },
        "scheduleTimeZone": {
          "type": "string",
          "metadata": {
            "Description": "Time zone for schedule."
          }
        },
        "scheduleLinkGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the schedule link to runbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the runbook job.",
            "control": "guid"
          }
        }
      },
      "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
    
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31",
    
        "Runbook": {
          "Name": "MyRunbook",
          "Description": "Sample runbook",
          "Type": "PowerShell",
          "Uri": "https://raw.githubusercontent.com/user/myrepo/master/samples/MyRunbook.ps1",
          "JobGuid": "[parameters('runbookJobGuid')]"
        },
    
        "Certificate": {
          "Name": "MyCertificate",
          "Base64Value": "[parameters('certificateBase64Value')]",
          "Thumbprint": "[parameters('certificateThumbprint')]"
        },
    
        "Credential": {
          "Name": "MyCredential",
          "UserName": "[parameters('credentialUsername')]",
          "Password": "[parameters('credentialPassword')]"
        },
    
        "Schedule": {
          "Name": "MySchedule",
          "Description": "Sample schedule",
          "IsEnabled": "true",
          "Interval": "1",
          "Frequency": "hour",
          "StartTime": "[parameters('scheduleStartTime')]",
          "TimeZone": "[parameters('scheduleTimeZone')]",
          "LinkGuid": "[parameters('scheduleLinkGuid')]"
        },
    
        "Variable": {
          "Name": "MyVariable",
          "Description": "Sample variable",
          "Encrypted": 0,
          "Type": "string",
          "Value": "'This is my string value.'"
        },
    
        "Module": {
          "Name": "OMSIngestionAPI",
          "Uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
        }
      },
      "resources": [
        {
          "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
          "location": "[parameters('workspaceRegionId')]",
          "tags": { },
          "type": "Microsoft.OperationsManagement/solutions",
          "apiVersion": "[variables('LogAnalyticsApiVersion')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
            "referencedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
            ],
            "containedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]"
            ]
          },
          "plan": {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
            "Version": "[variables('SolutionVersion')]",
            "product": "[variables('ProductName')]",
            "publisher": "[variables('SolutionPublisher')]",
            "promotionCode": ""
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
              "uri": "[variables('Runbook').Uri]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').JobGuid)]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('Runbook').Name]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
          "type": "Microsoft.Automation/automationAccounts/certificates",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "Base64Value": "[variables('Certificate').Base64Value]",
            "Thumbprint": "[variables('Certificate').Thumbprint]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
          "type": "Microsoft.Automation/automationAccounts/credentials",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "userName": "[variables('Credential').UserName]",
            "password": "[variables('Credential').Password]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
          "type": "microsoft.automation/automationAccounts/schedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Schedule').Description]",
            "startTime": "[variables('Schedule').StartTime]",
            "timeZone": "[variables('Schedule').TimeZone]",
            "isEnabled": "[variables('Schedule').IsEnabled]",
            "interval": "[variables('Schedule').Interval]",
            "frequency": "[variables('Schedule').Frequency]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
          "type": "microsoft.automation/automationAccounts/jobSchedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
          ],
          "tags": {
          },
          "properties": {
            "schedule": {
              "name": "[variables('Schedule').Name]"
            },
            "runbook": {
              "name": "[variables('Runbook').Name]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
          "type": "microsoft.automation/automationAccounts/variables",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Variable').Description]",
            "isEncrypted": "[variables('Variable').Encrypted]",
            "type": "[variables('Variable').Type]",
            "value": "[variables('Variable').Value]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
          "type": "Microsoft.Automation/automationAccounts/modules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "properties": {
            "contentLink": {
              "uri": "[variables('Module').Uri]"
            }
          }
        }
    
      ],
      "outputs": { }
    }




## <a name="next-steps"></a><span data-ttu-id="2d18a-301">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2d18a-301">Next steps</span></span>
* <span data-ttu-id="2d18a-302">[Lägga till en vy i lösningen](operations-management-suite-solutions-resources-views.md) visualisera insamlade data.</span><span class="sxs-lookup"><span data-stu-id="2d18a-302">[Add a view to your solution](operations-management-suite-solutions-resources-views.md) to visualize collected data.</span></span>
