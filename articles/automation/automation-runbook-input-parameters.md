---
title: aaaRunbook indataparametrar | Microsoft Docs
description: "Indataparametrarna för Runbook öka hello flexibilitet runbooks genom att låta dig toopass data tooa runbook när den startas. Den här artikeln beskrivs olika scenarier där indataparametrar används i runbooks."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 4d3dff2c-1f55-498d-9a0e-eee497e5bedb
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sngun
ms.openlocfilehash: f3abaf92382e7d41019616bafb14af23cf98dd9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-input-parameters"></a><span data-ttu-id="23997-104">Indataparametrar för Runbook</span><span class="sxs-lookup"><span data-stu-id="23997-104">Runbook input parameters</span></span>
<span data-ttu-id="23997-105">Indataparametrarna för Runbook öka hello flexibilitet runbooks genom att låta dig toopass data tooit när den startas.</span><span class="sxs-lookup"><span data-stu-id="23997-105">Runbook input parameters increase hello flexibility of runbooks by allowing you toopass data tooit when it is started.</span></span> <span data-ttu-id="23997-106">hello-parametrar kan hello runbook åtgärder toobe mål för specifika scenarier och miljöer.</span><span class="sxs-lookup"><span data-stu-id="23997-106">hello parameters allow hello runbook actions toobe targeted for specific scenarios and environments.</span></span> <span data-ttu-id="23997-107">I den här artikeln går du igenom olika scenarier där indataparametrar används i runbooks.</span><span class="sxs-lookup"><span data-stu-id="23997-107">In this article, we will walk you through different scenarios where input parameters are used in runbooks.</span></span>

## <a name="configure-input-parameters"></a><span data-ttu-id="23997-108">Konfigurera parametrar för indata</span><span class="sxs-lookup"><span data-stu-id="23997-108">Configure input parameters</span></span>
<span data-ttu-id="23997-109">Indataparametrar kan konfigureras i PowerShell och PowerShell-arbetsflöde grafiska runbook-flöden.</span><span class="sxs-lookup"><span data-stu-id="23997-109">Input parameters can be configured in PowerShell, PowerShell Workflow, and graphical runbooks.</span></span> <span data-ttu-id="23997-110">En runbook kan ha flera parametrar med olika datatyper eller inga parametrar alls.</span><span class="sxs-lookup"><span data-stu-id="23997-110">A runbook can have multiple parameters with different data types, or no parameters at all.</span></span> <span data-ttu-id="23997-111">Ange parametrarna kan vara obligatoriska eller valfria, och du kan tilldela ett standardvärde för valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="23997-111">Input parameters can be mandatory or optional, and you can assign a default value for optional parameters.</span></span> <span data-ttu-id="23997-112">Du kan tilldela värden toohello indataparametrar för en runbook när du startar det genom en hello tillgängliga metoder.</span><span class="sxs-lookup"><span data-stu-id="23997-112">You can assign values toohello input parameters for a runbook when you start it through one of hello available methods.</span></span> <span data-ttu-id="23997-113">De här metoderna omfattar att starta en runbook från hello-portalen eller en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="23997-113">These methods include starting  a runbook from hello portal  or a web service.</span></span> <span data-ttu-id="23997-114">Du kan också starta som en underordnad runbook som anropas infogad i en annan runbook.</span><span class="sxs-lookup"><span data-stu-id="23997-114">You can also start one as a child runbook that is called inline in another runbook.</span></span>

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a><span data-ttu-id="23997-115">Konfigurera indataparametrar i runbooks med PowerShell och PowerShell-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="23997-115">Configure input parameters in PowerShell and PowerShell Workflow runbooks</span></span>
<span data-ttu-id="23997-116">PowerShell och [PowerShell-arbetsflöde runbooks](automation-first-runbook-textual.md) stöder indataparametrar som definierats via hello följande attribut i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="23997-116">PowerShell and [PowerShell Workflow runbooks](automation-first-runbook-textual.md) in Azure Automation support input parameters that are defined through hello following attributes.</span></span>  

| <span data-ttu-id="23997-117">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="23997-117">**Property**</span></span> | <span data-ttu-id="23997-118">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="23997-118">**Description**</span></span> |
|:--- |:--- |
| <span data-ttu-id="23997-119">Typ</span><span class="sxs-lookup"><span data-stu-id="23997-119">Type</span></span> |<span data-ttu-id="23997-120">Krävs.</span><span class="sxs-lookup"><span data-stu-id="23997-120">Required.</span></span> <span data-ttu-id="23997-121">hello-datatyp som förväntas för hello parametervärde.</span><span class="sxs-lookup"><span data-stu-id="23997-121">hello data type expected for hello parameter value.</span></span> <span data-ttu-id="23997-122">.NET-typ är giltig.</span><span class="sxs-lookup"><span data-stu-id="23997-122">Any .NET type is valid.</span></span> |
| <span data-ttu-id="23997-123">Namn</span><span class="sxs-lookup"><span data-stu-id="23997-123">Name</span></span> |<span data-ttu-id="23997-124">Krävs.</span><span class="sxs-lookup"><span data-stu-id="23997-124">Required.</span></span> <span data-ttu-id="23997-125">hello namnet på hello-parameter.</span><span class="sxs-lookup"><span data-stu-id="23997-125">hello name of hello parameter.</span></span> <span data-ttu-id="23997-126">Detta måste vara unika inom hello runbook och kan bara innehålla bokstäver, siffror eller understreck.</span><span class="sxs-lookup"><span data-stu-id="23997-126">This must be unique within hello runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="23997-127">Det måste börja med en bokstav.</span><span class="sxs-lookup"><span data-stu-id="23997-127">It must start with a letter.</span></span> |
| <span data-ttu-id="23997-128">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="23997-128">Mandatory</span></span> |<span data-ttu-id="23997-129">Valfri.</span><span class="sxs-lookup"><span data-stu-id="23997-129">Optional.</span></span> <span data-ttu-id="23997-130">Anger om ett värde måste anges för hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="23997-130">Specifies whether a value must be provided for hello parameter.</span></span> <span data-ttu-id="23997-131">Om du väljer för**$true**, och sedan ett värde måste anges när hello runbook startas.</span><span class="sxs-lookup"><span data-stu-id="23997-131">If you set this too**$true**, then a value must be provided when hello runbook is started.</span></span> <span data-ttu-id="23997-132">Om du väljer för**$false**, och sedan ett värde är valfritt.</span><span class="sxs-lookup"><span data-stu-id="23997-132">If you set this too**$false**, then a value is optional.</span></span> |
| <span data-ttu-id="23997-133">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="23997-133">Default value</span></span> |<span data-ttu-id="23997-134">Valfri.</span><span class="sxs-lookup"><span data-stu-id="23997-134">Optional.</span></span>  <span data-ttu-id="23997-135">Anger ett värde som ska användas för hello-parametern som ett värde inte skickas när hello runbook startas.</span><span class="sxs-lookup"><span data-stu-id="23997-135">Specifies a value that will be used for hello parameter if a value is not passed in when hello runbook is started.</span></span> <span data-ttu-id="23997-136">Ett standardvärde kan anges för alla parametrar och görs automatiskt hello parametern valfria oavsett hello obligatoriska inställningen.</span><span class="sxs-lookup"><span data-stu-id="23997-136">A default value can be set for any parameter and will automatically make hello parameter optional regardless of hello Mandatory setting.</span></span> |

<span data-ttu-id="23997-137">Windows PowerShell stöder flera attribut för indataparametrarna än de som anges här, t.ex validering, alias, och parametern anger.</span><span class="sxs-lookup"><span data-stu-id="23997-137">Windows PowerShell supports more attributes of input parameters than those listed here, like validation, aliases, and parameter sets.</span></span> <span data-ttu-id="23997-138">Azure Automation stöder för närvarande endast hello indataparametrar som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="23997-138">However, Azure Automation currently supports only hello input parameters listed above.</span></span>

<span data-ttu-id="23997-139">En parameterdefinition i PowerShell-arbetsflöde runbooks har hello följande allmänna formulär där flera parametrar avgränsas med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="23997-139">A parameter definition in PowerShell Workflow runbooks has hello following general form, where multiple parameters are separated by commas.</span></span>

   ```
     Param
     (
         [Parameter (Mandatory= $true/$false)]
         [Type] Name1 = <Default value>,

         [Parameter (Mandatory= $true/$false)]
         [Type] Name2 = <Default value>
     )
   ```

> [!NOTE]
> <span data-ttu-id="23997-140">När du definierar parametrar, om du inte anger hello **obligatoriska** attribut, och sedan som standard hello parametern anses vara valfri.</span><span class="sxs-lookup"><span data-stu-id="23997-140">When you're defining parameters, if you don’t specify hello **Mandatory** attribute, then by default, hello parameter is considered optional.</span></span> <span data-ttu-id="23997-141">Även om du anger ett standardvärde för en parameter i PowerShell-arbetsflöde runbooks behandlas av PowerShell som en valfri parameter, oavsett hello **obligatoriska** attributvärdet.</span><span class="sxs-lookup"><span data-stu-id="23997-141">Also, if you set a default value for a parameter in PowerShell Workflow runbooks, it will be treated by PowerShell as an optional parameter, regardless of hello **Mandatory** attribute value.</span></span>
> 
> 

<span data-ttu-id="23997-142">Exempelvis ska vi konfigurera hello indataparametrar för en PowerShell-arbetsflödesrunbook som matar ut information om virtuella datorer, en enda virtuell dator eller alla virtuella datorer inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="23997-142">As an example, let’s configure hello input parameters for a PowerShell Workflow runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="23997-143">Denna runbook innehåller två parametrar som visas i följande skärmbild hello: hello namnet på virtuell dator och hello namnet på hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="23997-143">This runbook has two parameters as shown in hello following screenshot: hello name of virtual machine and hello name of hello resource group.</span></span>

![Automation PowerShell-arbetsflöde](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

<span data-ttu-id="23997-145">I den här parameterdefinitionen hello parametrar **$VMName** och **$resourceGroupName** är enkla parametrar av typen string.</span><span class="sxs-lookup"><span data-stu-id="23997-145">In this parameter definition, hello parameters **$VMName** and **$resourceGroupName** are simple parameters of type string.</span></span> <span data-ttu-id="23997-146">PowerShell och PowerShell-arbetsflöde runbooks stöder dock alla enkla typer och komplexa typer som **objekt** eller **PSCredential** för indataparametrarna.</span><span class="sxs-lookup"><span data-stu-id="23997-146">However, PowerShell and PowerShell Workflow runbooks support all simple types and complex types, such as **object** or **PSCredential** for input parameters.</span></span>

<span data-ttu-id="23997-147">Om din runbook har ett objekt Indataparametern, toopass i ett värde-par som sedan använda en PowerShell hash-tabell med (namn, värde).</span><span class="sxs-lookup"><span data-stu-id="23997-147">If your runbook has an object type input parameter, then use a PowerShell hashtable with (name,value) pairs toopass in a value.</span></span> <span data-ttu-id="23997-148">Till exempel om du har följande parameter i en runbook hello:</span><span class="sxs-lookup"><span data-stu-id="23997-148">For example, if you have hello following parameter in a runbook:</span></span>

     [Parameter (Mandatory = $true)]
     [object] $FullName

<span data-ttu-id="23997-149">Du kan sedan överföra hello följande värde toohello parameter:</span><span class="sxs-lookup"><span data-stu-id="23997-149">Then you can pass hello following value toohello parameter:</span></span>

    @{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}


## <a name="configure-input-parameters-in-graphical-runbooks"></a><span data-ttu-id="23997-150">Konfigurera indataparametrar i grafiska runbook-flöden</span><span class="sxs-lookup"><span data-stu-id="23997-150">Configure input parameters in graphical runbooks</span></span>
<span data-ttu-id="23997-151">för[konfigurerar en grafisk runbook](automation-first-runbook-graphical.md) med indataparametrar, skapar vi en grafisk runbook som matar ut information om virtuella datorer, antingen en enskild virtuell dator eller alla virtuella datorer inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="23997-151">too[configure a graphical runbook](automation-first-runbook-graphical.md) with input parameters, let’s create a graphical runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="23997-152">Konfigurera en runbook består av två viktiga aktiviteter som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="23997-152">Configuring a runbook consists of two major activities, as described below.</span></span>

<span data-ttu-id="23997-153">[**Autentisera Runbooks med Kör som-kontot Azure** ](automation-sec-configure-azure-runas-account.md) tooauthenticate med Azure.</span><span class="sxs-lookup"><span data-stu-id="23997-153">[**Authenticate Runbooks with Azure Run As account**](automation-sec-configure-azure-runas-account.md) tooauthenticate with Azure.</span></span>

<span data-ttu-id="23997-154">[**Get-AzureRmVm** ](https://msdn.microsoft.com/library/mt603718.aspx) tooget hello egenskaperna för en virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="23997-154">[**Get-AzureRmVm**](https://msdn.microsoft.com/library/mt603718.aspx) tooget hello properties of a virtual machines.</span></span>

<span data-ttu-id="23997-155">Du kan använda hello [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) aktivitetsnamn toooutput hello av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="23997-155">You can use hello [**Write-Output**](https://technet.microsoft.com/library/hh849921.aspx) activity toooutput hello names of virtual machines.</span></span> <span data-ttu-id="23997-156">Hej aktiviteten **Get-AzureRmVm** accepterar två parametrar hello **virtuellt datornamn** och hello **resursgruppens namn**.</span><span class="sxs-lookup"><span data-stu-id="23997-156">hello activity **Get-AzureRmVm** accepts two parameters, hello **virtual machine name** and hello **resource group name**.</span></span> <span data-ttu-id="23997-157">Eftersom de här parametrarna kan kräver olika värden varje gång du startar hello runbook kan du lägga till indataparametrar tooyour runbook.</span><span class="sxs-lookup"><span data-stu-id="23997-157">Since these parameters could require different values each time you start hello runbook, you can add input parameters tooyour runbook.</span></span> <span data-ttu-id="23997-158">Här följer stegen hello tooadd indataparametrar:</span><span class="sxs-lookup"><span data-stu-id="23997-158">Here are hello steps tooadd input parameters:</span></span>

1. <span data-ttu-id="23997-159">Välj hello grafisk runbook från hello **Runbooks** bladet och klicka sedan på [ **redigera** ](automation-graphical-authoring-intro.md) den.</span><span class="sxs-lookup"><span data-stu-id="23997-159">Select hello graphical runbook from hello **Runbooks** blade and then click [**Edit**](automation-graphical-authoring-intro.md) it.</span></span>
2. <span data-ttu-id="23997-160">Hej runbook-redigeraren, klicka på **ingående och utgående** tooopen hello **ingående och utgående** bladet.</span><span class="sxs-lookup"><span data-stu-id="23997-160">From hello runbook editor, click **Input and output** tooopen hello **Input and output** blade.</span></span>
   
    ![Grafisk Automation-runbook](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. <span data-ttu-id="23997-162">Hej **ingående och utgående** bladet visar en lista över indataparametrar som definierats för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="23997-162">hello **Input and output** blade displays a list of input parameters that are defined for hello runbook.</span></span> <span data-ttu-id="23997-163">På det här bladet kan du lägga till en ny Indataparametern eller redigera hello konfigurationen av en befintlig indataparameter.</span><span class="sxs-lookup"><span data-stu-id="23997-163">On this blade, you can either add a new input parameter or edit hello configuration of an existing input parameter.</span></span> <span data-ttu-id="23997-164">tooadd en ny parameter för hello runbook, klickar du på **lägga till indata** tooopen hello **runbookinmatningsparameter** bladet.</span><span class="sxs-lookup"><span data-stu-id="23997-164">tooadd a new parameter for hello runbook, click **Add input** tooopen hello **Runbook input parameter** blade.</span></span> <span data-ttu-id="23997-165">Där kan konfigurera du hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="23997-165">There, you can configure hello following parameters:</span></span>
   
   | <span data-ttu-id="23997-166">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="23997-166">**Property**</span></span> | <span data-ttu-id="23997-167">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="23997-167">**Description**</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="23997-168">Namn</span><span class="sxs-lookup"><span data-stu-id="23997-168">Name</span></span> |<span data-ttu-id="23997-169">Krävs.</span><span class="sxs-lookup"><span data-stu-id="23997-169">Required.</span></span>  <span data-ttu-id="23997-170">hello namnet på hello-parameter.</span><span class="sxs-lookup"><span data-stu-id="23997-170">hello name of hello parameter.</span></span> <span data-ttu-id="23997-171">Detta måste vara unika inom hello runbook och kan bara innehålla bokstäver, siffror eller understreck.</span><span class="sxs-lookup"><span data-stu-id="23997-171">This must be unique within hello runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="23997-172">Det måste börja med en bokstav.</span><span class="sxs-lookup"><span data-stu-id="23997-172">It must start with a letter.</span></span> |
   | <span data-ttu-id="23997-173">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="23997-173">Description</span></span> |<span data-ttu-id="23997-174">Valfri.</span><span class="sxs-lookup"><span data-stu-id="23997-174">Optional.</span></span> <span data-ttu-id="23997-175">Beskrivning av hello syftet indataparameter.</span><span class="sxs-lookup"><span data-stu-id="23997-175">Description about hello purpose of input parameter.</span></span> |
   | <span data-ttu-id="23997-176">Typ</span><span class="sxs-lookup"><span data-stu-id="23997-176">Type</span></span> |<span data-ttu-id="23997-177">Valfri.</span><span class="sxs-lookup"><span data-stu-id="23997-177">Optional.</span></span> <span data-ttu-id="23997-178">hello-datatyp som förväntas för hello parametervärde.</span><span class="sxs-lookup"><span data-stu-id="23997-178">hello data type that's expected for hello parameter value.</span></span> <span data-ttu-id="23997-179">Parametrar som stöds är **sträng**, **Int32**, **Int64**, **Decimal**, **booleskt**,  **DateTime**, och **objektet**.</span><span class="sxs-lookup"><span data-stu-id="23997-179">Supported parameter types are **String**, **Int32**, **Int64**, **Decimal**, **Boolean**, **DateTime**, and **Object**.</span></span> <span data-ttu-id="23997-180">Om datatypen inte är markerat används som standard för**sträng**.</span><span class="sxs-lookup"><span data-stu-id="23997-180">If a data type is not selected, it defaults too**String**.</span></span> |
   | <span data-ttu-id="23997-181">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="23997-181">Mandatory</span></span> |<span data-ttu-id="23997-182">Valfri.</span><span class="sxs-lookup"><span data-stu-id="23997-182">Optional.</span></span> <span data-ttu-id="23997-183">Anger om ett värde måste anges för hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="23997-183">Specifies whether a value must be provided for hello parameter.</span></span> <span data-ttu-id="23997-184">Om du väljer **Ja**, och sedan ett värde måste anges när hello runbook startas.</span><span class="sxs-lookup"><span data-stu-id="23997-184">If you choose **yes**, then a value must be provided when hello runbook is started.</span></span> <span data-ttu-id="23997-185">Om du väljer **inga**, och sedan ett värde inte krävs när hello runbook startas och du kan endast ange ett standardvärde.</span><span class="sxs-lookup"><span data-stu-id="23997-185">If you choose **no**, then a value is not required when hello runbook is started, and a default value may be set.</span></span> |
   | <span data-ttu-id="23997-186">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="23997-186">Default Value</span></span> |<span data-ttu-id="23997-187">Valfri.</span><span class="sxs-lookup"><span data-stu-id="23997-187">Optional.</span></span> <span data-ttu-id="23997-188">Anger ett värde som ska användas för hello-parametern som ett värde inte skickas när hello runbook startas.</span><span class="sxs-lookup"><span data-stu-id="23997-188">Specifies a value that will be used for hello parameter if a value is not passed in when hello runbook is started.</span></span> <span data-ttu-id="23997-189">Ett standardvärde kan anges för en parameter som inte är obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="23997-189">A default value can be set for a parameter that's not mandatory.</span></span> <span data-ttu-id="23997-190">tooset ett standardvärde, Välj **anpassad**.</span><span class="sxs-lookup"><span data-stu-id="23997-190">tooset a default value, choose **Custom**.</span></span> <span data-ttu-id="23997-191">Det här värdet används om inte ett annat värde anges när hello runbook startas.</span><span class="sxs-lookup"><span data-stu-id="23997-191">This value is used unless another value is provided when hello runbook is started.</span></span> <span data-ttu-id="23997-192">Välj **ingen** om du inte vill tooprovide något standardvärde.</span><span class="sxs-lookup"><span data-stu-id="23997-192">Choose **None** if you don’t want tooprovide any default value.</span></span> |
   
    ![Lägga till nya indata](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. <span data-ttu-id="23997-194">Skapa två parametrar med följande egenskaper kommer att användas av hello hello **Get-AzureRmVm** aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="23997-194">Create two parameters with hello following properties that will be used by hello **Get-AzureRmVm** activity:</span></span>
   
   * <span data-ttu-id="23997-195">**Parameter1:**</span><span class="sxs-lookup"><span data-stu-id="23997-195">**Parameter1:**</span></span>
     
     * <span data-ttu-id="23997-196">Namn - VMName</span><span class="sxs-lookup"><span data-stu-id="23997-196">Name - VMName</span></span>
     * <span data-ttu-id="23997-197">Typ - sträng</span><span class="sxs-lookup"><span data-stu-id="23997-197">Type - String</span></span>
     * <span data-ttu-id="23997-198">Obligatoriska - Nej</span><span class="sxs-lookup"><span data-stu-id="23997-198">Mandatory - No</span></span>
   * <span data-ttu-id="23997-199">**Parameter2:**</span><span class="sxs-lookup"><span data-stu-id="23997-199">**Parameter2:**</span></span>
     
     * <span data-ttu-id="23997-200">Namn - resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="23997-200">Name - resourceGroupName</span></span>
     * <span data-ttu-id="23997-201">Typ - sträng</span><span class="sxs-lookup"><span data-stu-id="23997-201">Type - String</span></span>
     * <span data-ttu-id="23997-202">Obligatoriska - Nej</span><span class="sxs-lookup"><span data-stu-id="23997-202">Mandatory - No</span></span>
     * <span data-ttu-id="23997-203">Standardvärde - anpassad</span><span class="sxs-lookup"><span data-stu-id="23997-203">Default value - Custom</span></span>
     * <span data-ttu-id="23997-204">Anpassade standardvärde - \<namnet på hello resursgruppen som innehåller hello virtuella datorer ></span><span class="sxs-lookup"><span data-stu-id="23997-204">Custom default value - \<Name of hello resource group that contains hello virtual machines></span></span>
5. <span data-ttu-id="23997-205">När du lägger till hello parametrar, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="23997-205">Once you add hello parameters, click **OK**.</span></span>  <span data-ttu-id="23997-206">Du kan nu visa dem i hello **indata och utdata bladet**.</span><span class="sxs-lookup"><span data-stu-id="23997-206">You can now view them in hello **Input and output blade**.</span></span> <span data-ttu-id="23997-207">Klicka på **OK** igen, och klicka sedan på **spara** och **publicera** din runbook.</span><span class="sxs-lookup"><span data-stu-id="23997-207">Click **OK** again, and then click **Save** and **Publish** your runbook.</span></span>

## <a name="assign-values-tooinput-parameters-in-runbooks"></a><span data-ttu-id="23997-208">Tilldela värden tooinput parametrar i runbooks</span><span class="sxs-lookup"><span data-stu-id="23997-208">Assign values tooinput parameters in runbooks</span></span>
<span data-ttu-id="23997-209">Du kan skicka värden tooinput parametrar i runbooks i hello följande scenarier.</span><span class="sxs-lookup"><span data-stu-id="23997-209">You can pass values tooinput parameters in runbooks in hello following scenarios.</span></span>

### <a name="start-a-runbook-and-assign-parameters"></a><span data-ttu-id="23997-210">Starta en runbook och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="23997-210">Start a runbook and assign parameters</span></span>
<span data-ttu-id="23997-211">En runbook kan startas på många olika sätt: genom hello Azure-portalen med en webhook, med PowerShell-cmdletar, med hello REST API eller med hello SDK.</span><span class="sxs-lookup"><span data-stu-id="23997-211">A runbook can be started many ways: through hello Azure portal, with a webhook, with PowerShell cmdlets, with hello REST API, or with hello SDK.</span></span> <span data-ttu-id="23997-212">Nedan diskuterar vi olika metoder för att starta en runbook och tilldela parametrar.</span><span class="sxs-lookup"><span data-stu-id="23997-212">Below we discuss different methods for starting a runbook and assigning parameters.</span></span>

#### <a name="start-a-published-runbook-by-using-hello-azure-portal-and-assign-parameters"></a><span data-ttu-id="23997-213">Starta en publicerad runbook med hjälp av hello Azure-portalen och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="23997-213">Start a published runbook by using hello Azure portal and assign parameters</span></span>
<span data-ttu-id="23997-214">När du [starta hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), hello **starta Runbook** blad öppnas och du kan konfigurera värden för hello-parametrar som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="23997-214">When you [start hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), hello **Start Runbook** blade opens and you can configure values for hello parameters that you just created.</span></span>

![Börja använda hello-portalen](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

<span data-ttu-id="23997-216">Du kan se hello-attribut som har angetts för parametern hello hello etikett under hello textrutan.</span><span class="sxs-lookup"><span data-stu-id="23997-216">In hello label beneath hello input box, you can see hello attributes that have been set for hello parameter.</span></span> <span data-ttu-id="23997-217">Attribut är obligatoriska eller valfria, typ och standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="23997-217">Attributes include mandatory or optional, type, and  default value.</span></span> <span data-ttu-id="23997-218">I hello hjälp pratbubblor nästa toohello parameternamn ser du alla hello nyckelinformation som du behöver toomake beslut om inkommande parametervärden.</span><span class="sxs-lookup"><span data-stu-id="23997-218">In hello help balloon next toohello parameter name, you can see all hello key information you need toomake decisions about parameter input values.</span></span> <span data-ttu-id="23997-219">Informationen omfattar om en parameter är obligatoriska eller valfria.</span><span class="sxs-lookup"><span data-stu-id="23997-219">This information includes whether a parameter is mandatory or optional.</span></span> <span data-ttu-id="23997-220">Den innehåller också hello typ och standardvärdet (eventuella) och annan användbar information.</span><span class="sxs-lookup"><span data-stu-id="23997-220">It also includes hello type and default value (if any), and other helpful notes.</span></span>

![Hjälp pratbubblor](media/automation-runbook-input-parameters/automation-05-helpbaloon.png)

> [!NOTE]
> <span data-ttu-id="23997-222">Stöd för parametrar av typen String **tom** String värden.</span><span class="sxs-lookup"><span data-stu-id="23997-222">String type parameters support **Empty** String values.</span></span>  <span data-ttu-id="23997-223">Ange **[EmptyString]** i hello Indataparametern rutan skickar en tom sträng toohello-parameter.</span><span class="sxs-lookup"><span data-stu-id="23997-223">Entering **[EmptyString]** in hello input parameter box will pass an empty string toohello parameter.</span></span> <span data-ttu-id="23997-224">Dessutom stöder inte typen strängparametrar **Null** värden som skickas.</span><span class="sxs-lookup"><span data-stu-id="23997-224">Also, String type parameters don’t support **Null** values being passed.</span></span> <span data-ttu-id="23997-225">Om du inte klarar alla värdet toohello strängparameter sedan tolkas PowerShell det som null.</span><span class="sxs-lookup"><span data-stu-id="23997-225">If you don’t pass any value toohello String parameter, then PowerShell will interpret it as null.</span></span>
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a><span data-ttu-id="23997-226">Starta en publicerad runbook med hjälp av PowerShell-cmdlets och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="23997-226">Start a published runbook by using PowerShell cmdlets and assign parameters</span></span>
* <span data-ttu-id="23997-227">**Azure Resource Manager-cmdlets:** du kan starta en Automation-runbook som har skapats i en resursgrupp med hjälp av [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span><span class="sxs-lookup"><span data-stu-id="23997-227">**Azure Resource Manager cmdlets:** You can start an Automation runbook that was created in a resource group by using [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span></span>
  
  <span data-ttu-id="23997-228">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="23997-228">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* <span data-ttu-id="23997-229">**Azure Service Management-cmdlets:** du kan starta en automation-runbook som har skapats i en standard-resursgrupp med hjälp av [Start AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span><span class="sxs-lookup"><span data-stu-id="23997-229">**Azure Service Management cmdlets:** You can start an automation runbook that was created in a default resource group by using [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span></span>
  
  <span data-ttu-id="23997-230">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="23997-230">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> <span data-ttu-id="23997-231">När du startar en runbook med hjälp av PowerShell-cmdletar, en standardparameter **MicrosoftApplicationManagementStartedBy** skapas med hello värdet **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="23997-231">When you start a runbook by using PowerShell cmdlets, a default parameter, **MicrosoftApplicationManagementStartedBy** is created with hello value **PowerShell**.</span></span> <span data-ttu-id="23997-232">Du kan visa den här parametern i hello **Jobbdetaljer** bladet.</span><span class="sxs-lookup"><span data-stu-id="23997-232">You can view this parameter in hello **Job details** blade.</span></span>  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a><span data-ttu-id="23997-233">Starta en runbook med hjälp av ett SDK och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="23997-233">Start a runbook by using an SDK and assign parameters</span></span>
* <span data-ttu-id="23997-234">**Azure Resource Manager-metod:** du kan starta en runbook med hjälp av hello SDK för ett programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="23997-234">**Azure Resource Manager method:** You can start a runbook by using hello SDK of a programming language.</span></span> <span data-ttu-id="23997-235">Nedan visas ett C# kodfragment för att starta en runbook i Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="23997-235">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="23997-236">Du kan visa alla hello kod i vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="23997-236">You can view all hello code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>  
  
  ```
   public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
      {
        var response = AutomationClient.Jobs.Create(resourceGroupName, automationAccount, new JobCreateParameters
         {
            Properties = new JobCreateProperties
             {
                Runbook = new RunbookAssociationProperty
                 {
                   Name = runbookName
                 },
                   Parameters = parameters
             }
         });
      return response.Job;
      }
  ```
* <span data-ttu-id="23997-237">**Azure Service Management-metod:** du kan starta en runbook med hjälp av hello SDK för ett programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="23997-237">**Azure Service Management method:** You can start a runbook by using hello SDK of a programming language.</span></span> <span data-ttu-id="23997-238">Nedan visas ett C# kodfragment för att starta en runbook i Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="23997-238">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="23997-239">Du kan visa alla hello kod i vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="23997-239">You can view all hello code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>
  
  ```      
  public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
    {
      var response = AutomationClient.Jobs.Create(automationAccount, new JobCreateParameters
    {
      Properties = new JobCreateProperties
         {
           Runbook = new RunbookAssociationProperty
         {
           Name = runbookName
              },
                Parameters = parameters
              }
       });
      return response.Job;
    }
  ```
  
  <span data-ttu-id="23997-240">toostart den här metoden skapar en ordlista toostore hello runbook-parametrar, **VMName** och **resourceGroupName**, och deras värden.</span><span class="sxs-lookup"><span data-stu-id="23997-240">toostart this method, create a dictionary toostore hello runbook parameters, **VMName** and  **resourceGroupName**, and their values.</span></span> <span data-ttu-id="23997-241">Starta hello runbook.</span><span class="sxs-lookup"><span data-stu-id="23997-241">Then start hello runbook.</span></span> <span data-ttu-id="23997-242">Nedan visas hello C# kodfragment för att anropa hello-metod som har definierats ovan.</span><span class="sxs-lookup"><span data-stu-id="23997-242">Below is hello C# code snippet for calling hello method that's defined above.</span></span>
  
  ```
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters toohello dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call hello StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-hello-rest-api-and-assign-parameters"></a><span data-ttu-id="23997-243">Starta en runbook med hjälp av hello REST-API och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="23997-243">Start a runbook by using hello REST API and assign parameters</span></span>
<span data-ttu-id="23997-244">Ett runbook-jobb kan skapas och igång med hello Azure Automation REST-API med hjälp av hello **PLACERA** metod med hello följande begärande-URI.</span><span class="sxs-lookup"><span data-stu-id="23997-244">A runbook job can be created and started with hello Azure Automation REST API by using hello **PUT** method with hello following request URI.</span></span>

    https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`

<span data-ttu-id="23997-245">I hello Begärd URI, ersätter du hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="23997-245">In hello request URI, replace hello following parameters:</span></span>

* <span data-ttu-id="23997-246">**prenumerations-id:** ditt Azure-prenumeration-ID.</span><span class="sxs-lookup"><span data-stu-id="23997-246">**subscription-id:** Your Azure subscription ID.</span></span>  
* <span data-ttu-id="23997-247">**molntjänstnamnet-:** hello namnet på hello molnet toowhich hello begäran ska skickas.</span><span class="sxs-lookup"><span data-stu-id="23997-247">**cloud-service-name:** hello name of hello cloud service toowhich hello request should be sent.</span></span>  
* <span data-ttu-id="23997-248">**Automation-kontonamn:** hello namnet på ditt automation-konto som ligger inom hello angivna Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="23997-248">**automation-account-name:** hello name of your automation account that's hosted within hello specified cloud service.</span></span>  
* <span data-ttu-id="23997-249">**jobb-id:** hello GUID för hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="23997-249">**job-id:** hello GUID for hello job.</span></span> <span data-ttu-id="23997-250">GUID i PowerShell kan skapas med hjälp av hello **[GUID]::NewGuid(). ToString()** kommando.</span><span class="sxs-lookup"><span data-stu-id="23997-250">GUIDs in PowerShell can be created by using hello **[GUID]::NewGuid().ToString()** command.</span></span>

<span data-ttu-id="23997-251">Använd hello begärandetexten i ordning toopass parametrar toohello runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="23997-251">In order toopass parameters toohello runbook job, use hello request body.</span></span> <span data-ttu-id="23997-252">Det tar hello följande två egenskaper i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="23997-252">It takes hello following two properties provided in JSON format:</span></span>

* <span data-ttu-id="23997-253">**Runbook-namn:** krävs.</span><span class="sxs-lookup"><span data-stu-id="23997-253">**Runbook name:** Required.</span></span> <span data-ttu-id="23997-254">hello namn på hello runbook hello jobbet toostart.</span><span class="sxs-lookup"><span data-stu-id="23997-254">hello name of hello runbook for hello job toostart.</span></span>  
* <span data-ttu-id="23997-255">**Runbook-parametrar:** valfritt.</span><span class="sxs-lookup"><span data-stu-id="23997-255">**Runbook parameters:** Optional.</span></span> <span data-ttu-id="23997-256">En ordlista med hello parameterlista i (namn, värde) format där namnet ska vara av typen sträng och värdet kan vara ett giltigt JSON-värde.</span><span class="sxs-lookup"><span data-stu-id="23997-256">A dictionary of hello parameter list in (name, value) format where name should be of String type and value can be any valid JSON value.</span></span>

<span data-ttu-id="23997-257">Om du vill toostart hello **Get-AzureVMTextual** runbook som har skapats tidigare med **VMName** och **resourceGroupName** som parametrar och använda hello följande JSON-format för hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="23997-257">If you want toostart hello **Get-AzureVMTextual** runbook that was created earlier with **VMName** and **resourceGroupName** as parameters, use hello following JSON format for hello request body.</span></span>

   ```
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WSVMClassic",
         "resourceGroupName":”WSCS1”}
        }
    }
   ```

<span data-ttu-id="23997-258">En HTTP-statuskod 201 returneras om hello jobbet har skapats.</span><span class="sxs-lookup"><span data-stu-id="23997-258">A HTTP status code 201 is returned if hello job is successfully created.</span></span> <span data-ttu-id="23997-259">Mer information om svarshuvuden och hello svarstexten finns toohello artikel om hur för[skapa ett runbook-jobb med hjälp av hello REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span><span class="sxs-lookup"><span data-stu-id="23997-259">For more information on response headers and hello response body, refer toohello article about how too[create a runbook job by using hello REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span></span>

### <a name="test-a-runbook-and-assign-parameters"></a><span data-ttu-id="23997-260">Testa en runbook och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="23997-260">Test a runbook and assign parameters</span></span>
<span data-ttu-id="23997-261">När du [test hello utkastversionen för din runbook](automation-testing-runbook.md) med alternativet hello test hello **testa** blad öppnas och du kan konfigurera värden för hello-parametrar som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="23997-261">When you [test hello draft version of your runbook](automation-testing-runbook.md) by using hello test option, hello **Test** blade opens and you can configure values for hello parameters that you just created.</span></span>

![Testa och tilldela parametrar](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-tooa-runbook-and-assign-parameters"></a><span data-ttu-id="23997-263">Länka ett schema tooa runbook och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="23997-263">Link a schedule tooa runbook and assign parameters</span></span>
<span data-ttu-id="23997-264">Du kan [länka ett schema](automation-schedules.md) tooyour runbook så att hello runbook startar vid en viss tid.</span><span class="sxs-lookup"><span data-stu-id="23997-264">You can [link a schedule](automation-schedules.md) tooyour runbook so that hello runbook starts at a specific time.</span></span> <span data-ttu-id="23997-265">Du kan tilldela indataparametrar när du skapar hello schema och hello runbook ska använda dessa värden när den startas efter hello schema.</span><span class="sxs-lookup"><span data-stu-id="23997-265">You assign input parameters when you create hello schedule, and hello runbook will use these values when it is started by hello schedule.</span></span> <span data-ttu-id="23997-266">Du kan inte spara hello schema förrän alla obligatoriska parametervärden har angetts.</span><span class="sxs-lookup"><span data-stu-id="23997-266">You can’t save hello schedule until all mandatory parameter values are provided.</span></span>

![Schemalägga och tilldela parametrar](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a><span data-ttu-id="23997-268">Skapa en webhook för en runbook och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="23997-268">Create a webhook for a runbook and assign parameters</span></span>
<span data-ttu-id="23997-269">Du kan skapa en [webhook](automation-webhooks.md) för din runbook och konfigurera indataparametrarna för runbook.</span><span class="sxs-lookup"><span data-stu-id="23997-269">You can create a [webhook](automation-webhooks.md) for your runbook and configure runbook input parameters.</span></span> <span data-ttu-id="23997-270">Du kan inte spara hello webhook förrän alla obligatoriska parametervärden har angetts.</span><span class="sxs-lookup"><span data-stu-id="23997-270">You can’t save hello webhook until all mandatory parameter values are provided.</span></span>

![Skapa webhook och tilldela parametrar](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

<span data-ttu-id="23997-272">När du kör en runbook med hjälp av en webhook hello fördefinierade Indataparametern  **[Webhookdata](automation-webhooks.md#details-of-a-webhook)**  skickas tillsammans med hello indataparametrar som du har definierat.</span><span class="sxs-lookup"><span data-stu-id="23997-272">When you execute a runbook by using a webhook, hello predefined input parameter **[Webhookdata](automation-webhooks.md#details-of-a-webhook)** is sent, along with hello input parameters that you defined.</span></span> <span data-ttu-id="23997-273">Du kan klicka på tooexpand hello **WebhookData** parameter för mer information.</span><span class="sxs-lookup"><span data-stu-id="23997-273">You can click tooexpand hello **WebhookData** parameter for more details.</span></span>

![WebhookData-parameter](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a><span data-ttu-id="23997-275">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23997-275">Next steps</span></span>
* <span data-ttu-id="23997-276">Mer information om runbook ingående och utgående finns [Azure Automation: runbook indata, utdata och kapslade runbooks](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span><span class="sxs-lookup"><span data-stu-id="23997-276">For more information on runbook input and output, see [Azure Automation: runbook input, output, and nested runbooks](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span></span>
* <span data-ttu-id="23997-277">Mer information om olika sätt toostart en runbook finns [starta en runbook](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="23997-277">For details about different ways toostart a runbook, see [Starting a runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="23997-278">tooedit en textrepresentation runbook finns för[redigera textrepresentation runbooks](automation-edit-textual-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="23997-278">tooedit a textual runbook, refer too[Editing textual runbooks](automation-edit-textual-runbook.md).</span></span>
* <span data-ttu-id="23997-279">tooedit en grafisk runbook finns för[grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="23997-279">tooedit a graphical runbook, refer too[Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md).</span></span>

