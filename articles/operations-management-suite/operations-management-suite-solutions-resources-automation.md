---
title: "aaaAzure Automation resurser i OMS-lösningar | Microsoft Docs"
description: "Lösningar i OMS inkluderar vanligtvis runbooks i Azure Automation tooautomate processer, till exempel att samla in och bearbetning av övervakningsdata.  Den här artikeln beskriver hur tooinclude runbooks och deras relaterade resurser i en lösning."
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
ms.openlocfilehash: 82a156f89bf77ce25e52e5e4596261ec07a24dae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-automation-resources-tooan-oms-management-solution-preview"></a><span data-ttu-id="cf057-104">Lägga till Azure Automation resurser tooan OMS hanteringslösning (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="cf057-104">Adding Azure Automation resources tooan OMS management solution (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="cf057-105">Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="cf057-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="cf057-106">Ett schema som beskrivs nedan är ämne toochange.</span><span class="sxs-lookup"><span data-stu-id="cf057-106">Any schema described below is subject toochange.</span></span>   


<span data-ttu-id="cf057-107">[Lösningar för hantering i OMS](operations-management-suite-solutions.md) inkluderar vanligtvis runbooks i Azure Automation tooautomate processer, till exempel att samla in och bearbetning av övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="cf057-107">[Management solutions in OMS](operations-management-suite-solutions.md) will typically include runbooks in Azure Automation tooautomate processes such as collecting and processing monitoring data.</span></span>  <span data-ttu-id="cf057-108">Dessutom toorunbooks, Automation-konton innehåller tillgångar som variabler och scheman som stöder hello runbooks som används i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="cf057-108">In addition toorunbooks, Automation accounts includes assets such as variables and schedules that support hello runbooks used in hello solution.</span></span>  <span data-ttu-id="cf057-109">Den här artikeln beskriver hur tooinclude runbooks och deras relaterade resurser i en lösning.</span><span class="sxs-lookup"><span data-stu-id="cf057-109">This article describes how tooinclude runbooks and their related resources in a solution.</span></span>

> [!NOTE]
> <span data-ttu-id="cf057-110">hello exempel i den här artikeln använder parametrar och variabler är antingen obligatorisk eller vanliga toomanagement lösningar som beskrivs i [och skapa lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="cf057-110">hello samples in this article use parameters and variables that are either required or common toomanagement solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="cf057-111">Krav</span><span class="sxs-lookup"><span data-stu-id="cf057-111">Prerequisites</span></span>
<span data-ttu-id="cf057-112">Den här artikeln förutsätter att du redan är bekant med hello följande information.</span><span class="sxs-lookup"><span data-stu-id="cf057-112">This article assumes that you're already familiar with hello following information.</span></span>

- <span data-ttu-id="cf057-113">Hur för[skapar en lösning för](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="cf057-113">How too[create a management solution](operations-management-suite-solutions-creating.md).</span></span>
- <span data-ttu-id="cf057-114">Hej struktur för en [lösningsfilen](operations-management-suite-solutions-solution-file.md).</span><span class="sxs-lookup"><span data-stu-id="cf057-114">hello structure of a [solution file](operations-management-suite-solutions-solution-file.md).</span></span>
- <span data-ttu-id="cf057-115">Hur för[skapar Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="cf057-115">How too[author Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md)</span></span>

## <a name="automation-account"></a><span data-ttu-id="cf057-116">Automation-konto</span><span class="sxs-lookup"><span data-stu-id="cf057-116">Automation account</span></span>
<span data-ttu-id="cf057-117">Alla resurser i Azure Automation finns i en [Automation-konto](../automation/automation-security-overview.md#automation-account-overview).</span><span class="sxs-lookup"><span data-stu-id="cf057-117">All resources in Azure Automation are contained in an [Automation account](../automation/automation-security-overview.md#automation-account-overview).</span></span>  <span data-ttu-id="cf057-118">Enligt beskrivningen i [OMS arbetsytan och Automation-konto](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello Automation-konto ingår inte i hello hanteringslösning men det måste finnas innan hello lösningen är installerad.</span><span class="sxs-lookup"><span data-stu-id="cf057-118">As described in [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello Automation account isn't included in hello management solution but must exist before hello solution is installed.</span></span>  <span data-ttu-id="cf057-119">Hello lösning installationen misslyckas om den inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="cf057-119">If it isn't available, then hello solution install will fail.</span></span>

<span data-ttu-id="cf057-120">hello ingår varje Automation resurs hello namnet på dess Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="cf057-120">hello name of each Automation resource includes hello name of its Automation account.</span></span>  <span data-ttu-id="cf057-121">Detta görs i hello lösningen med hello **accountName** parameter som hello följande exempel på en runbook-resurs.</span><span class="sxs-lookup"><span data-stu-id="cf057-121">This is done in hello solution with hello **accountName** parameter as in hello following example of a runbook resource.</span></span>

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a><span data-ttu-id="cf057-122">Runbooks</span><span class="sxs-lookup"><span data-stu-id="cf057-122">Runbooks</span></span>
<span data-ttu-id="cf057-123">Du bör innehålla alla runbooks som används av hello lösning i hello lösningsfilen så att de skapas när hello lösningen är installerad.</span><span class="sxs-lookup"><span data-stu-id="cf057-123">You should include any runbooks used by hello solution in hello solution file so that they're created when hello solution is installed.</span></span>  <span data-ttu-id="cf057-124">Du får inte innehålla hello brödtext hello runbook i hello mallen, så du bör publicera hello runbook tooa allmän plats där den kan nås av alla användare som installerar din lösning.</span><span class="sxs-lookup"><span data-stu-id="cf057-124">You cannot contain hello body of hello runbook in hello template though, so you should publish hello runbook tooa public location where it can be accessed by any user installing your solution.</span></span>

<span data-ttu-id="cf057-125">[Azure Automation-runbook](../automation/automation-runbook-types.md) resurser har en typ av **Microsoft.Automation/automationAccounts/runbooks** och ha hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="cf057-125">[Azure Automation runbook](../automation/automation-runbook-types.md) resources have a type of **Microsoft.Automation/automationAccounts/runbooks** and have hello following structure.</span></span> <span data-ttu-id="cf057-126">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="cf057-126">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

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


<span data-ttu-id="cf057-127">hello i den följande tabellen beskrivs hello egenskaper för runbooks.</span><span class="sxs-lookup"><span data-stu-id="cf057-127">hello properties for runbooks are described in hello following table.</span></span>

| <span data-ttu-id="cf057-128">Egenskap</span><span class="sxs-lookup"><span data-stu-id="cf057-128">Property</span></span> | <span data-ttu-id="cf057-129">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cf057-129">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cf057-130">runbookType</span><span class="sxs-lookup"><span data-stu-id="cf057-130">runbookType</span></span> |<span data-ttu-id="cf057-131">Anger hello hello runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-131">Specifies hello types of hello runbook.</span></span> <br><br> <span data-ttu-id="cf057-132">Skript - PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="cf057-132">Script - PowerShell script</span></span> <br><span data-ttu-id="cf057-133">PowerShell - PowerShell-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="cf057-133">PowerShell - PowerShell workflow</span></span> <br> <span data-ttu-id="cf057-134">GraphPowerShell - grafisk PowerShell-skriptet runbook</span><span class="sxs-lookup"><span data-stu-id="cf057-134">GraphPowerShell - Graphical PowerShell script runbook</span></span> <br> <span data-ttu-id="cf057-135">GraphPowerShellWorkflow - grafisk PowerShell-arbetsflödesrunbook</span><span class="sxs-lookup"><span data-stu-id="cf057-135">GraphPowerShellWorkflow - Graphical PowerShell workflow runbook</span></span> |
| <span data-ttu-id="cf057-136">logProgress</span><span class="sxs-lookup"><span data-stu-id="cf057-136">logProgress</span></span> |<span data-ttu-id="cf057-137">Anger om [vidare poster](../automation/automation-runbook-output-and-messages.md) ska genereras för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-137">Specifies whether [progress records](../automation/automation-runbook-output-and-messages.md) should be generated for hello runbook.</span></span> |
| <span data-ttu-id="cf057-138">logVerbose</span><span class="sxs-lookup"><span data-stu-id="cf057-138">logVerbose</span></span> |<span data-ttu-id="cf057-139">Anger om [utförliga poster](../automation/automation-runbook-output-and-messages.md) ska genereras för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-139">Specifies whether [verbose records](../automation/automation-runbook-output-and-messages.md) should be generated for hello runbook.</span></span> |
| <span data-ttu-id="cf057-140">description</span><span class="sxs-lookup"><span data-stu-id="cf057-140">description</span></span> |<span data-ttu-id="cf057-141">Valfri beskrivning för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-141">Optional description for hello runbook.</span></span> |
| <span data-ttu-id="cf057-142">publishContentLink</span><span class="sxs-lookup"><span data-stu-id="cf057-142">publishContentLink</span></span> |<span data-ttu-id="cf057-143">Anger hello innehållet i hello runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-143">Specifies hello content of hello runbook.</span></span> <br><br><span data-ttu-id="cf057-144">URI - Uri toohello innehållet i hello runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-144">uri - Uri toohello content of hello runbook.</span></span>  <span data-ttu-id="cf057-145">Det här är en .ps1-fil för PowerShell-skript och runbooks och en exporterade grafiska runbook-fil för en grafisk runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-145">This will be a .ps1 file for PowerShell and Script runbooks, and an exported graphical runbook file for a Graph runbook.</span></span>  <br> <span data-ttu-id="cf057-146">version - versionen av hello runbook för egna spårning.</span><span class="sxs-lookup"><span data-stu-id="cf057-146">version - Version of hello runbook for your own tracking.</span></span> |


## <a name="automation-jobs"></a><span data-ttu-id="cf057-147">Automation-jobb</span><span class="sxs-lookup"><span data-stu-id="cf057-147">Automation jobs</span></span>
<span data-ttu-id="cf057-148">När du startar en runbook i Azure Automation skapas ett automation-jobb.</span><span class="sxs-lookup"><span data-stu-id="cf057-148">When you start a runbook in Azure Automation, it creates an automation job.</span></span>  <span data-ttu-id="cf057-149">Du kan lägga till en automation resurs tooyour lösning tooautomatically jobbstart en runbook när hello hanteringslösning installeras.</span><span class="sxs-lookup"><span data-stu-id="cf057-149">You can add an automation job resource tooyour solution tooautomatically start a runbook when hello management solution is installed.</span></span>  <span data-ttu-id="cf057-150">Den här metoden är brukar användas toostart runbooks som används för inledande konfiguration av hello lösning.</span><span class="sxs-lookup"><span data-stu-id="cf057-150">This method is typically used toostart runbooks that are used for initial configuration of hello solution.</span></span>  <span data-ttu-id="cf057-151">toostart en runbook med jämna mellanrum, skapa en [schema](#schedules) och en [Jobbschema](#job-schedules)</span><span class="sxs-lookup"><span data-stu-id="cf057-151">toostart a runbook at regular intervals, create a [schedule](#schedules) and a [job schedule](#job-schedules)</span></span>

<span data-ttu-id="cf057-152">Resurser för jobbet har en typ av **Microsoft.Automation/automationAccounts/jobs** och ha hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="cf057-152">Job resources have a type of **Microsoft.Automation/automationAccounts/jobs** and have hello following structure.</span></span>  <span data-ttu-id="cf057-153">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="cf057-153">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

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

<span data-ttu-id="cf057-154">hello egenskaper för automation-jobb beskrivs i följande tabell hello.</span><span class="sxs-lookup"><span data-stu-id="cf057-154">hello properties for automation jobs are described in hello following table.</span></span>

| <span data-ttu-id="cf057-155">Egenskap</span><span class="sxs-lookup"><span data-stu-id="cf057-155">Property</span></span> | <span data-ttu-id="cf057-156">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cf057-156">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cf057-157">runbook</span><span class="sxs-lookup"><span data-stu-id="cf057-157">runbook</span></span> |<span data-ttu-id="cf057-158">Enda namn entitet med hello namnet på hello runbook toostart.</span><span class="sxs-lookup"><span data-stu-id="cf057-158">Single name entity with hello name of hello runbook toostart.</span></span> |
| <span data-ttu-id="cf057-159">parameters</span><span class="sxs-lookup"><span data-stu-id="cf057-159">parameters</span></span> |<span data-ttu-id="cf057-160">Entitet för varje parametervärde som krävs av hello runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-160">Entity for each parameter value required by hello runbook.</span></span> |

<span data-ttu-id="cf057-161">hello jobbet innehåller hello runbook-namn och eventuella parametern värden toobe skickas toohello runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-161">hello job includes hello runbook name and any parameter values toobe sent toohello runbook.</span></span>  <span data-ttu-id="cf057-162">hello jobb bör [beror på](operations-management-suite-solutions-solution-file.md#resources) hello-runbook som startar sedan hello runbook måste skapas innan hello jobb.</span><span class="sxs-lookup"><span data-stu-id="cf057-162">hello job should [depend on](operations-management-suite-solutions-solution-file.md#resources) hello runbook that it's starting since hello runbook must be created before hello job.</span></span>  <span data-ttu-id="cf057-163">Om du har flera runbooks som ska startas kan du definiera deras inbördes ordning genom att ett jobb är beroende av andra jobb som ska köras första.</span><span class="sxs-lookup"><span data-stu-id="cf057-163">If you have multiple runbooks that should be started you can define their order by having a job depend on any other jobs that should be run first.</span></span>

<span data-ttu-id="cf057-164">hello namnet på en resurs för jobbet måste innehålla en GUID som tilldelas vanligtvis av en parameter.</span><span class="sxs-lookup"><span data-stu-id="cf057-164">hello name of a job resource must contain a GUID which is typically assigned by a parameter.</span></span>  <span data-ttu-id="cf057-165">Du kan läsa mer om GUID-parametrar i [och skapa lösningar i Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="cf057-165">You can read more about GUID parameters in [Creating solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).</span></span>  


## <a name="certificates"></a><span data-ttu-id="cf057-166">Certifikat</span><span class="sxs-lookup"><span data-stu-id="cf057-166">Certificates</span></span>
<span data-ttu-id="cf057-167">[Azure Automation-certifikat](../automation/automation-certificates.md) har en typ av **Microsoft.Automation/automationAccounts/certificates** och ha hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="cf057-167">[Azure Automation certificates](../automation/automation-certificates.md) have a type of **Microsoft.Automation/automationAccounts/certificates** and have hello following structure.</span></span> <span data-ttu-id="cf057-168">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="cf057-168">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

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



<span data-ttu-id="cf057-169">hello egenskaper för certifikat resurser beskrivs i följande tabell hello.</span><span class="sxs-lookup"><span data-stu-id="cf057-169">hello properties for Certificates resources are described in hello following table.</span></span>

| <span data-ttu-id="cf057-170">Egenskap</span><span class="sxs-lookup"><span data-stu-id="cf057-170">Property</span></span> | <span data-ttu-id="cf057-171">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cf057-171">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cf057-172">base64Value</span><span class="sxs-lookup"><span data-stu-id="cf057-172">base64Value</span></span> |<span data-ttu-id="cf057-173">Base 64-värde för hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="cf057-173">Base 64 value for hello certificate.</span></span> |
| <span data-ttu-id="cf057-174">tumavtrycket</span><span class="sxs-lookup"><span data-stu-id="cf057-174">thumbprint</span></span> |<span data-ttu-id="cf057-175">Tumavtryck för certifikat som hello.</span><span class="sxs-lookup"><span data-stu-id="cf057-175">Thumbprint for hello certificate.</span></span> |



## <a name="credentials"></a><span data-ttu-id="cf057-176">Autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="cf057-176">Credentials</span></span>
<span data-ttu-id="cf057-177">[Autentiseringsuppgifter för Azure Automation](../automation/automation-credentials.md) har en typ av **Microsoft.Automation/automationAccounts/credentials** och ha hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="cf057-177">[Azure Automation credentials](../automation/automation-credentials.md) have a type of **Microsoft.Automation/automationAccounts/credentials** and have hello following structure.</span></span>  <span data-ttu-id="cf057-178">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="cf057-178">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 


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

<span data-ttu-id="cf057-179">i hello i den följande tabellen beskrivs hello egenskaper för autentiseringsuppgifter resurser.</span><span class="sxs-lookup"><span data-stu-id="cf057-179">hello properties for Credential resources are described in hello following table.</span></span>

| <span data-ttu-id="cf057-180">Egenskap</span><span class="sxs-lookup"><span data-stu-id="cf057-180">Property</span></span> | <span data-ttu-id="cf057-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cf057-181">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cf057-182">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="cf057-182">userName</span></span> |<span data-ttu-id="cf057-183">Användarnamn för hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cf057-183">User name for hello credential.</span></span> |
| <span data-ttu-id="cf057-184">lösenord</span><span class="sxs-lookup"><span data-stu-id="cf057-184">password</span></span> |<span data-ttu-id="cf057-185">Lösenordet för hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cf057-185">Password for hello credential.</span></span> |


## <a name="schedules"></a><span data-ttu-id="cf057-186">Scheman</span><span class="sxs-lookup"><span data-stu-id="cf057-186">Schedules</span></span>
<span data-ttu-id="cf057-187">[Azure Automation-scheman](../automation/automation-schedules.md) har en typ av **Microsoft.Automation/automationAccounts/schedules** och ha hello hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="cf057-187">[Azure Automation schedules](../automation/automation-schedules.md) have a type of **Microsoft.Automation/automationAccounts/schedules** and have hello hello following structure.</span></span> <span data-ttu-id="cf057-188">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="cf057-188">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

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

<span data-ttu-id="cf057-189">hello egenskaper för schema resurser beskrivs i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="cf057-189">hello properties for schedule resources are described in hello following table.</span></span>

| <span data-ttu-id="cf057-190">Egenskap</span><span class="sxs-lookup"><span data-stu-id="cf057-190">Property</span></span> | <span data-ttu-id="cf057-191">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cf057-191">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cf057-192">description</span><span class="sxs-lookup"><span data-stu-id="cf057-192">description</span></span> |<span data-ttu-id="cf057-193">Valfri beskrivning för hello schema.</span><span class="sxs-lookup"><span data-stu-id="cf057-193">Optional description for hello schedule.</span></span> |
| <span data-ttu-id="cf057-194">startTime</span><span class="sxs-lookup"><span data-stu-id="cf057-194">startTime</span></span> |<span data-ttu-id="cf057-195">Anger hello starttiden för ett schema som ett DateTime-objekt.</span><span class="sxs-lookup"><span data-stu-id="cf057-195">Specifies hello start time of a schedule as a DateTime object.</span></span> <span data-ttu-id="cf057-196">En sträng kan tillhandahållas om det kan vara konverterade tooa giltigt DateTime.</span><span class="sxs-lookup"><span data-stu-id="cf057-196">A string can be provided if it can be converted tooa valid DateTime.</span></span> |
| <span data-ttu-id="cf057-197">IsEnabled</span><span class="sxs-lookup"><span data-stu-id="cf057-197">isEnabled</span></span> |<span data-ttu-id="cf057-198">Anger om hello schema har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="cf057-198">Specifies whether hello schedule is enabled.</span></span> |
| <span data-ttu-id="cf057-199">interval</span><span class="sxs-lookup"><span data-stu-id="cf057-199">interval</span></span> |<span data-ttu-id="cf057-200">hello typ av intervall för hello schema.</span><span class="sxs-lookup"><span data-stu-id="cf057-200">hello type of interval for hello schedule.</span></span><br><br><span data-ttu-id="cf057-201">dag</span><span class="sxs-lookup"><span data-stu-id="cf057-201">day</span></span><br><span data-ttu-id="cf057-202">timme</span><span class="sxs-lookup"><span data-stu-id="cf057-202">hour</span></span> |
| <span data-ttu-id="cf057-203">frequency</span><span class="sxs-lookup"><span data-stu-id="cf057-203">frequency</span></span> |<span data-ttu-id="cf057-204">Frekvens som hello schemat ska utlösas i antal dagar eller timmar.</span><span class="sxs-lookup"><span data-stu-id="cf057-204">Frequency that hello schedule should fire in number of days or hours.</span></span> |

<span data-ttu-id="cf057-205">Måste ha en starttid med ett större värde än hello aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="cf057-205">Schedules must have a start time with a value greater than hello current time.</span></span>  <span data-ttu-id="cf057-206">Du kan inte ange det här värdet med en variabel eftersom du har ingen möjlighet att veta när den är pågående toobe installerad.</span><span class="sxs-lookup"><span data-stu-id="cf057-206">You cannot provide this value with a variable since you would have no way of knowing when it's going toobe installed.</span></span>

<span data-ttu-id="cf057-207">Använd någon av följande två olika metoder när du använder schemalägga resurser i en lösning hello.</span><span class="sxs-lookup"><span data-stu-id="cf057-207">Use one of hello following two strategies when using schedule resources in a solution.</span></span>

- <span data-ttu-id="cf057-208">Använda en parameter för hello starttiden för hello schema.</span><span class="sxs-lookup"><span data-stu-id="cf057-208">Use a parameter for hello start time of hello schedule.</span></span>  <span data-ttu-id="cf057-209">Då hello användaren tooprovide ett värde när de installerar hello lösning.</span><span class="sxs-lookup"><span data-stu-id="cf057-209">This will prompt hello user tooprovide a value when they install hello solution.</span></span>  <span data-ttu-id="cf057-210">Om du har flera scheman, kan du använda en enda parameter-värdet för fler än ett.</span><span class="sxs-lookup"><span data-stu-id="cf057-210">If you have multiple schedules, you could use a single parameter value for more than one of them.</span></span>
- <span data-ttu-id="cf057-211">Skapa hello scheman med hjälp av en runbook som startar när hello lösningen är installerad.</span><span class="sxs-lookup"><span data-stu-id="cf057-211">Create hello schedules using a runbook that starts when hello solution is installed.</span></span>  <span data-ttu-id="cf057-212">Detta tar bort hello behovet av hello användaren toospecify en gång, men du inte kan innehålla hello schema i din lösning så tas den bort när hello lösningen tas bort.</span><span class="sxs-lookup"><span data-stu-id="cf057-212">This removes hello requirement of hello user toospecify a time, but you can't contain hello schedule in your solution so it will be removed when hello solution is removed.</span></span>


### <a name="job-schedules"></a><span data-ttu-id="cf057-213">Jobbscheman</span><span class="sxs-lookup"><span data-stu-id="cf057-213">Job schedules</span></span>
<span data-ttu-id="cf057-214">Jobbet schemalägga resurser länkar en runbook med ett schema.</span><span class="sxs-lookup"><span data-stu-id="cf057-214">Job schedule resources link a runbook with a schedule.</span></span>  <span data-ttu-id="cf057-215">De har en typ av **Microsoft.Automation/automationAccounts/jobSchedules** och ha hello hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="cf057-215">They have a type of **Microsoft.Automation/automationAccounts/jobSchedules** and have hello hello following structure.</span></span>  <span data-ttu-id="cf057-216">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="cf057-216">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

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


<span data-ttu-id="cf057-217">hello egenskaper för jobbscheman beskrivs i hello i den följande tabellen.</span><span class="sxs-lookup"><span data-stu-id="cf057-217">hello properties for job schedules are described in hello following table.</span></span>

| <span data-ttu-id="cf057-218">Egenskap</span><span class="sxs-lookup"><span data-stu-id="cf057-218">Property</span></span> | <span data-ttu-id="cf057-219">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cf057-219">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cf057-220">namn på schema</span><span class="sxs-lookup"><span data-stu-id="cf057-220">schedule name</span></span> |<span data-ttu-id="cf057-221">Enskild **namn** entitet med hello namnet på hello schema.</span><span class="sxs-lookup"><span data-stu-id="cf057-221">Single **name** entity with hello name of hello schedule.</span></span> |
| <span data-ttu-id="cf057-222">Runbook-namn</span><span class="sxs-lookup"><span data-stu-id="cf057-222">runbook name</span></span>  |<span data-ttu-id="cf057-223">Enskild **namn** entitet med hello namnet på hello runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-223">Single **name** entity with hello name of hello runbook.</span></span>  |



## <a name="variables"></a><span data-ttu-id="cf057-224">Variabler</span><span class="sxs-lookup"><span data-stu-id="cf057-224">Variables</span></span>
<span data-ttu-id="cf057-225">[Azure Automation-variabler](../automation/automation-variables.md) har en typ av **Microsoft.Automation/automationAccounts/variables** och ha hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="cf057-225">[Azure Automation variables](../automation/automation-variables.md) have a type of **Microsoft.Automation/automationAccounts/variables** and have hello following structure.</span></span>  <span data-ttu-id="cf057-226">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="cf057-226">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span>

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

<span data-ttu-id="cf057-227">hello egenskaper för variabeln resurser beskrivs i följande tabell hello.</span><span class="sxs-lookup"><span data-stu-id="cf057-227">hello properties for variable resources are described in hello following table.</span></span>

| <span data-ttu-id="cf057-228">Egenskap</span><span class="sxs-lookup"><span data-stu-id="cf057-228">Property</span></span> | <span data-ttu-id="cf057-229">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cf057-229">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cf057-230">description</span><span class="sxs-lookup"><span data-stu-id="cf057-230">description</span></span> | <span data-ttu-id="cf057-231">Valfri beskrivning för hello-variabeln.</span><span class="sxs-lookup"><span data-stu-id="cf057-231">Optional description for hello variable.</span></span> |
| <span data-ttu-id="cf057-232">isEncrypted</span><span class="sxs-lookup"><span data-stu-id="cf057-232">isEncrypted</span></span> | <span data-ttu-id="cf057-233">Anger om hello variabeln ska krypteras.</span><span class="sxs-lookup"><span data-stu-id="cf057-233">Specifies whether hello variable should be encrypted.</span></span> |
| <span data-ttu-id="cf057-234">typ</span><span class="sxs-lookup"><span data-stu-id="cf057-234">type</span></span> | <span data-ttu-id="cf057-235">Den här egenskapen har för närvarande ingen effekt.</span><span class="sxs-lookup"><span data-stu-id="cf057-235">This property currently has no effect.</span></span>  <span data-ttu-id="cf057-236">hello datatypen för variabel hello bestäms av hello startvärde.</span><span class="sxs-lookup"><span data-stu-id="cf057-236">hello data type of hello variable will be determined by hello initial value.</span></span> |
| <span data-ttu-id="cf057-237">värde</span><span class="sxs-lookup"><span data-stu-id="cf057-237">value</span></span> | <span data-ttu-id="cf057-238">Värdet för hello variabel.</span><span class="sxs-lookup"><span data-stu-id="cf057-238">Value for hello variable.</span></span> |

> [!NOTE]
> <span data-ttu-id="cf057-239">Hej **typen** egenskapen för närvarande har ingen effekt på hello variabel skapas.</span><span class="sxs-lookup"><span data-stu-id="cf057-239">hello **type** property currently has no effect on hello variable being created.</span></span>  <span data-ttu-id="cf057-240">hello datatyp för hello variabel bestäms av hello värde.</span><span class="sxs-lookup"><span data-stu-id="cf057-240">hello data type for hello variable will be determined by hello value.</span></span>  

<span data-ttu-id="cf057-241">Om du ställer in hello ursprungligt värde för variabeln hello måste den konfigureras som hello rätt datatyp.</span><span class="sxs-lookup"><span data-stu-id="cf057-241">If you set hello initial value for hello variable, it must be configured as hello correct data type.</span></span>  <span data-ttu-id="cf057-242">hello följande tabell innehåller hello olika datatyper tillåten och deras syntax.</span><span class="sxs-lookup"><span data-stu-id="cf057-242">hello following table provides hello different data types allowable and their syntax.</span></span>  <span data-ttu-id="cf057-243">Observera att värdena i JSON förväntade tooalways stå inom citattecken med några specialtecken inom hello citattecken.</span><span class="sxs-lookup"><span data-stu-id="cf057-243">Note that values in JSON are expected tooalways be enclosed in quotes with any special characters within hello quotes.</span></span>  <span data-ttu-id="cf057-244">Till exempel ett strängvärde skulle anges med citattecken runt hello sträng (med hjälp av hello escape-tecken (\\)) när ett numeriskt värde skulle anges med en uppsättning av citattecken.</span><span class="sxs-lookup"><span data-stu-id="cf057-244">For example, a string value would be specified by quotes around hello string (using hello escape character (\\)) while a numeric value would be specified with one set of quotes.</span></span>

| <span data-ttu-id="cf057-245">Datatyp</span><span class="sxs-lookup"><span data-stu-id="cf057-245">Data type</span></span> | <span data-ttu-id="cf057-246">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cf057-246">Description</span></span> | <span data-ttu-id="cf057-247">Exempel</span><span class="sxs-lookup"><span data-stu-id="cf057-247">Example</span></span> | <span data-ttu-id="cf057-248">Löser för</span><span class="sxs-lookup"><span data-stu-id="cf057-248">Resolves too</span></span>|
|:--|:--|:--|:--|
| <span data-ttu-id="cf057-249">Sträng</span><span class="sxs-lookup"><span data-stu-id="cf057-249">string</span></span>   | <span data-ttu-id="cf057-250">Värdet omges av dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="cf057-250">Enclose value in double quotes.</span></span>  | <span data-ttu-id="cf057-251">”\"Hello world\"”</span><span class="sxs-lookup"><span data-stu-id="cf057-251">"\"Hello world\""</span></span> | <span data-ttu-id="cf057-252">”Hello world”</span><span class="sxs-lookup"><span data-stu-id="cf057-252">"Hello world"</span></span> |
| <span data-ttu-id="cf057-253">numeriskt</span><span class="sxs-lookup"><span data-stu-id="cf057-253">numeric</span></span>  | <span data-ttu-id="cf057-254">Numeriskt värde med enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="cf057-254">Numeric value with single quotes.</span></span>| <span data-ttu-id="cf057-255">"64"</span><span class="sxs-lookup"><span data-stu-id="cf057-255">"64"</span></span> | <span data-ttu-id="cf057-256">64</span><span class="sxs-lookup"><span data-stu-id="cf057-256">64</span></span> |
| <span data-ttu-id="cf057-257">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="cf057-257">boolean</span></span>  | <span data-ttu-id="cf057-258">**SANT** eller **FALSKT** inom citattecken.</span><span class="sxs-lookup"><span data-stu-id="cf057-258">**true** or **false** in quotes.</span></span>  <span data-ttu-id="cf057-259">Observera att det här värdet måste vara gemener.</span><span class="sxs-lookup"><span data-stu-id="cf057-259">Note that this value must be lowercase.</span></span> | <span data-ttu-id="cf057-260">”true”</span><span class="sxs-lookup"><span data-stu-id="cf057-260">"true"</span></span> | <span data-ttu-id="cf057-261">SANT</span><span class="sxs-lookup"><span data-stu-id="cf057-261">true</span></span> |
| <span data-ttu-id="cf057-262">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="cf057-262">datetime</span></span> | <span data-ttu-id="cf057-263">Serialiserad date-värde.</span><span class="sxs-lookup"><span data-stu-id="cf057-263">Serialized date value.</span></span><br><span data-ttu-id="cf057-264">Du kan använda hello ConvertTo Json-cmdlet i PowerShell toogenerate värdet för ett visst datum.</span><span class="sxs-lookup"><span data-stu-id="cf057-264">You can use hello ConvertTo-Json cmdlet in PowerShell toogenerate this value for a particular date.</span></span><br><span data-ttu-id="cf057-265">Exempel: get-date ”2017-5/24 13:14:57” \\</span><span class="sxs-lookup"><span data-stu-id="cf057-265">Example: get-date "5/24/2017 13:14:57" \\</span></span>| <span data-ttu-id="cf057-266">ConvertTo Json</span><span class="sxs-lookup"><span data-stu-id="cf057-266">ConvertTo-Json</span></span> | <span data-ttu-id="cf057-267">”\\/Date(1495656897378)\\/”</span><span class="sxs-lookup"><span data-stu-id="cf057-267">"\\/Date(1495656897378)\\/"</span></span> | <span data-ttu-id="cf057-268">2017-05-24 13:14:57</span><span class="sxs-lookup"><span data-stu-id="cf057-268">2017-05-24 13:14:57</span></span> |

## <a name="modules"></a><span data-ttu-id="cf057-269">Moduler</span><span class="sxs-lookup"><span data-stu-id="cf057-269">Modules</span></span>
<span data-ttu-id="cf057-270">Management-lösningen behöver inte toodefine [global modul](../automation/automation-integration-modules.md) används av dina runbooks eftersom de alltid är tillgängliga i ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="cf057-270">Your management solution does not need toodefine [global modules](../automation/automation-integration-modules.md) used by your runbooks because they will always be available in your Automation account.</span></span>  <span data-ttu-id="cf057-271">Du behöver tooinclude en resurs för alla moduler som används av dina runbooks.</span><span class="sxs-lookup"><span data-stu-id="cf057-271">You do need tooinclude a resource for any other module used by your runbooks.</span></span>

<span data-ttu-id="cf057-272">[Integreringsmoduler](../automation/automation-integration-modules.md) har en typ av **Microsoft.Automation/automationAccounts/modules** och ha hello följande struktur.</span><span class="sxs-lookup"><span data-stu-id="cf057-272">[Integration modules](../automation/automation-integration-modules.md) have a type of **Microsoft.Automation/automationAccounts/modules** and have hello following structure.</span></span>  <span data-ttu-id="cf057-273">Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.</span><span class="sxs-lookup"><span data-stu-id="cf057-273">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span>

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


<span data-ttu-id="cf057-274">i hello i den följande tabellen beskrivs hello egenskaper för modulen resurser.</span><span class="sxs-lookup"><span data-stu-id="cf057-274">hello properties for module resources are described in hello following table.</span></span>

| <span data-ttu-id="cf057-275">Egenskap</span><span class="sxs-lookup"><span data-stu-id="cf057-275">Property</span></span> | <span data-ttu-id="cf057-276">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cf057-276">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cf057-277">contentLink</span><span class="sxs-lookup"><span data-stu-id="cf057-277">contentLink</span></span> |<span data-ttu-id="cf057-278">Anger hello innehållet i hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="cf057-278">Specifies hello content of hello module.</span></span> <br><br><span data-ttu-id="cf057-279">URI - Uri toohello innehållet i hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="cf057-279">uri - Uri toohello content of hello module.</span></span>  <span data-ttu-id="cf057-280">Det här är en .ps1-fil för PowerShell-skript och runbooks och en exporterade grafiska runbook-fil för en grafisk runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-280">This will be a .ps1 file for PowerShell and Script runbooks, and an exported graphical runbook file for a Graph runbook.</span></span>  <br> <span data-ttu-id="cf057-281">version - Version av hello-modulen för egna spårning.</span><span class="sxs-lookup"><span data-stu-id="cf057-281">version - Version of hello module for your own tracking.</span></span> |

<span data-ttu-id="cf057-282">Hej runbook ska beror på hello modulen resurs tooensure som har skapats innan hello runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-282">hello runbook should depend on hello module resource tooensure that it's created before hello runbook.</span></span>

### <a name="updating-modules"></a><span data-ttu-id="cf057-283">Uppdatera moduler</span><span class="sxs-lookup"><span data-stu-id="cf057-283">Updating modules</span></span>
<span data-ttu-id="cf057-284">Om du uppdaterar en lösning som innehåller en runbook som använder ett schema och hello ny version av lösningen har en ny modul som används av denna runbook, använda hello runbook hello gammal version av hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="cf057-284">If you update a management solution that includes a runbook that uses a schedule, and hello new version of your solution has a new module used by that runbook, then hello runbook may use hello old version of hello module.</span></span>  <span data-ttu-id="cf057-285">Du bör inkludera hello följande runbooks i din lösning och skapa ett jobb toorun dem före andra runbooks.</span><span class="sxs-lookup"><span data-stu-id="cf057-285">You should include hello following runbooks in your solution and create a job toorun them before any other runbooks.</span></span>  <span data-ttu-id="cf057-286">Se till att alla moduler som är uppdaterade som krävs innan hello runbooks har lästs in.</span><span class="sxs-lookup"><span data-stu-id="cf057-286">This will ensure that any modules are updated as required before hello runbooks are loaded.</span></span>

* <span data-ttu-id="cf057-287">[Uppdatera ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) säkerställer att alla hello-moduler som används av runbooks i din lösning är hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="cf057-287">[Update-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) will ensure that all of hello modules used by runbooks in your solution are hello latest version.</span></span>  
* <span data-ttu-id="cf057-288">[ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) ska registrera om alla hello schema resurser tooensure att hello runbooks kopplade toothem med Använd hello senaste moduler.</span><span class="sxs-lookup"><span data-stu-id="cf057-288">[ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) will reregister all of hello schedule resources tooensure that hello runbooks linked toothem with use hello latest modules.</span></span>




## <a name="sample"></a><span data-ttu-id="cf057-289">Exempel</span><span class="sxs-lookup"><span data-stu-id="cf057-289">Sample</span></span>
<span data-ttu-id="cf057-290">Följande är ett exempel på en lösning som omfattar som innehåller hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="cf057-290">Following is a sample of a solution that include that includes hello following resources:</span></span>

- <span data-ttu-id="cf057-291">Runbook.</span><span class="sxs-lookup"><span data-stu-id="cf057-291">Runbook.</span></span>  <span data-ttu-id="cf057-292">Det här är en exempel-runbook som lagras i en offentlig GitHub-databas.</span><span class="sxs-lookup"><span data-stu-id="cf057-292">This is a sample runbook stored in a public GitHub repository.</span></span>
- <span data-ttu-id="cf057-293">Automation-jobb som startar hello runbook när hello lösningen är installerad.</span><span class="sxs-lookup"><span data-stu-id="cf057-293">Automation job that starts hello runbook when hello solution is installed.</span></span>
- <span data-ttu-id="cf057-294">Schemat och jobb kan du schemalägga toostart hello runbook med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="cf057-294">Schedule and job schedule toostart hello runbook at regular intervals.</span></span>
- <span data-ttu-id="cf057-295">Certifikat.</span><span class="sxs-lookup"><span data-stu-id="cf057-295">Certificate.</span></span>
- <span data-ttu-id="cf057-296">Autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cf057-296">Credential.</span></span>
- <span data-ttu-id="cf057-297">Variabel.</span><span class="sxs-lookup"><span data-stu-id="cf057-297">Variable.</span></span>
- <span data-ttu-id="cf057-298">Modul.</span><span class="sxs-lookup"><span data-stu-id="cf057-298">Module.</span></span>  <span data-ttu-id="cf057-299">Detta är hello [OMSIngestionAPI modulen](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) för att skriva data tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="cf057-299">This is hello [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) for writing data tooLog Analytics.</span></span> 

<span data-ttu-id="cf057-300">Hej exempel använder [standardlösningen parametrar](operations-management-suite-solutions-solution-file.md#parameters) variabler som ofta används i en lösning som motsats toohardcoding värdena i resursdefinitionerna hello.</span><span class="sxs-lookup"><span data-stu-id="cf057-300">hello sample uses [standard solution parameters](operations-management-suite-solutions-solution-file.md#parameters) variables that would commonly be used in a solution as opposed toohardcoding values in hello resource definitions.</span></span>


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
            "description": "GUID for hello schedule link toorunbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for hello runbook job.",
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




## <a name="next-steps"></a><span data-ttu-id="cf057-301">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cf057-301">Next steps</span></span>
* <span data-ttu-id="cf057-302">[Lägg till en vy tooyour lösning](operations-management-suite-solutions-resources-views.md) toovisualize insamlade data.</span><span class="sxs-lookup"><span data-stu-id="cf057-302">[Add a view tooyour solution](operations-management-suite-solutions-resources-views.md) toovisualize collected data.</span></span>
