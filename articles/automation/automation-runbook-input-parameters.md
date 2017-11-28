---
title: "Indataparametrar för Runbook | Microsoft Docs"
description: "Indataparametrarna för Runbook ökas flexibiliteten för runbooks genom att du kan skicka data till en runbook när den startas. Den här artikeln beskrivs olika scenarier där indataparametrar används i runbooks."
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
ms.openlocfilehash: 1ebf32338f5242e72eb2e2daa2da50d231f4b683
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="runbook-input-parameters"></a><span data-ttu-id="7b78e-104">Indataparametrar för Runbook</span><span class="sxs-lookup"><span data-stu-id="7b78e-104">Runbook input parameters</span></span>
<span data-ttu-id="7b78e-105">Indataparametrarna för Runbook ökas flexibiliteten för runbooks genom att du kan skicka data till den när den startas.</span><span class="sxs-lookup"><span data-stu-id="7b78e-105">Runbook input parameters increase the flexibility of runbooks by allowing you to pass data to it when it is started.</span></span> <span data-ttu-id="7b78e-106">Parametrarna gör runbook-åtgärder kan vara mål för specifika scenarier och miljöer.</span><span class="sxs-lookup"><span data-stu-id="7b78e-106">The parameters allow the runbook actions to be targeted for specific scenarios and environments.</span></span> <span data-ttu-id="7b78e-107">I den här artikeln går du igenom olika scenarier där indataparametrar används i runbooks.</span><span class="sxs-lookup"><span data-stu-id="7b78e-107">In this article, we will walk you through different scenarios where input parameters are used in runbooks.</span></span>

## <a name="configure-input-parameters"></a><span data-ttu-id="7b78e-108">Konfigurera parametrar för indata</span><span class="sxs-lookup"><span data-stu-id="7b78e-108">Configure input parameters</span></span>
<span data-ttu-id="7b78e-109">Indataparametrar kan konfigureras i PowerShell och PowerShell-arbetsflöde grafiska runbook-flöden.</span><span class="sxs-lookup"><span data-stu-id="7b78e-109">Input parameters can be configured in PowerShell, PowerShell Workflow, and graphical runbooks.</span></span> <span data-ttu-id="7b78e-110">En runbook kan ha flera parametrar med olika datatyper eller inga parametrar alls.</span><span class="sxs-lookup"><span data-stu-id="7b78e-110">A runbook can have multiple parameters with different data types, or no parameters at all.</span></span> <span data-ttu-id="7b78e-111">Ange parametrarna kan vara obligatoriska eller valfria, och du kan tilldela ett standardvärde för valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="7b78e-111">Input parameters can be mandatory or optional, and you can assign a default value for optional parameters.</span></span> <span data-ttu-id="7b78e-112">Du kan tilldela värden indataparametrar för en runbook när du startar det genom en av de tillgängliga metoderna.</span><span class="sxs-lookup"><span data-stu-id="7b78e-112">You can assign values to the input parameters for a runbook when you start it through one of the available methods.</span></span> <span data-ttu-id="7b78e-113">De här metoderna omfattar att starta en runbook från portalen eller en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="7b78e-113">These methods include starting  a runbook from the portal  or a web service.</span></span> <span data-ttu-id="7b78e-114">Du kan också starta som en underordnad runbook som anropas infogad i en annan runbook.</span><span class="sxs-lookup"><span data-stu-id="7b78e-114">You can also start one as a child runbook that is called inline in another runbook.</span></span>

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a><span data-ttu-id="7b78e-115">Konfigurera indataparametrar i runbooks med PowerShell och PowerShell-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="7b78e-115">Configure input parameters in PowerShell and PowerShell Workflow runbooks</span></span>
<span data-ttu-id="7b78e-116">PowerShell och [PowerShell-arbetsflöde runbooks](automation-first-runbook-textual.md) stöder indataparametrar som definieras med följande attribut i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="7b78e-116">PowerShell and [PowerShell Workflow runbooks](automation-first-runbook-textual.md) in Azure Automation support input parameters that are defined through the following attributes.</span></span>  

| <span data-ttu-id="7b78e-117">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="7b78e-117">**Property**</span></span> | <span data-ttu-id="7b78e-118">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="7b78e-118">**Description**</span></span> |
|:--- |:--- |
| <span data-ttu-id="7b78e-119">Typ</span><span class="sxs-lookup"><span data-stu-id="7b78e-119">Type</span></span> |<span data-ttu-id="7b78e-120">Krävs.</span><span class="sxs-lookup"><span data-stu-id="7b78e-120">Required.</span></span> <span data-ttu-id="7b78e-121">Datatypen för värdet för parametern.</span><span class="sxs-lookup"><span data-stu-id="7b78e-121">The data type expected for the parameter value.</span></span> <span data-ttu-id="7b78e-122">.NET-typ är giltig.</span><span class="sxs-lookup"><span data-stu-id="7b78e-122">Any .NET type is valid.</span></span> |
| <span data-ttu-id="7b78e-123">Namn</span><span class="sxs-lookup"><span data-stu-id="7b78e-123">Name</span></span> |<span data-ttu-id="7b78e-124">Krävs.</span><span class="sxs-lookup"><span data-stu-id="7b78e-124">Required.</span></span> <span data-ttu-id="7b78e-125">Parameterns namn.</span><span class="sxs-lookup"><span data-stu-id="7b78e-125">The name of the parameter.</span></span> <span data-ttu-id="7b78e-126">Detta måste vara unika inom en runbook och kan bara innehålla bokstäver, siffror eller understreck.</span><span class="sxs-lookup"><span data-stu-id="7b78e-126">This must be unique within the runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="7b78e-127">Det måste börja med en bokstav.</span><span class="sxs-lookup"><span data-stu-id="7b78e-127">It must start with a letter.</span></span> |
| <span data-ttu-id="7b78e-128">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="7b78e-128">Mandatory</span></span> |<span data-ttu-id="7b78e-129">Valfri.</span><span class="sxs-lookup"><span data-stu-id="7b78e-129">Optional.</span></span> <span data-ttu-id="7b78e-130">Anger om ett värde måste anges för parametern.</span><span class="sxs-lookup"><span data-stu-id="7b78e-130">Specifies whether a value must be provided for the parameter.</span></span> <span data-ttu-id="7b78e-131">Om du väljer **$true**, och sedan ett värde måste anges när runbooken startar.</span><span class="sxs-lookup"><span data-stu-id="7b78e-131">If you set this to **$true**, then a value must be provided when the runbook is started.</span></span> <span data-ttu-id="7b78e-132">Om du väljer **$false**, och sedan ett värde är valfritt.</span><span class="sxs-lookup"><span data-stu-id="7b78e-132">If you set this to **$false**, then a value is optional.</span></span> |
| <span data-ttu-id="7b78e-133">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="7b78e-133">Default value</span></span> |<span data-ttu-id="7b78e-134">Valfri.</span><span class="sxs-lookup"><span data-stu-id="7b78e-134">Optional.</span></span>  <span data-ttu-id="7b78e-135">Anger ett värde som ska användas för parametern som ett värde inte skickas när runbook startas.</span><span class="sxs-lookup"><span data-stu-id="7b78e-135">Specifies a value that will be used for the parameter if a value is not passed in when the runbook is started.</span></span> <span data-ttu-id="7b78e-136">Ett standardvärde kan anges för alla parametrar och görs automatiskt parametern valfria oavsett inställningen obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="7b78e-136">A default value can be set for any parameter and will automatically make the parameter optional regardless of the Mandatory setting.</span></span> |

<span data-ttu-id="7b78e-137">Windows PowerShell stöder flera attribut för indataparametrarna än de som anges här, t.ex validering, alias, och parametern anger.</span><span class="sxs-lookup"><span data-stu-id="7b78e-137">Windows PowerShell supports more attributes of input parameters than those listed here, like validation, aliases, and parameter sets.</span></span> <span data-ttu-id="7b78e-138">Azure Automation stöder för närvarande endast indataparametrar som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="7b78e-138">However, Azure Automation currently supports only the input parameters listed above.</span></span>

<span data-ttu-id="7b78e-139">En parameterdefinition i PowerShell-arbetsflöde runbooks har följande allmänna formuläret, där flera parametrar avgränsas med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="7b78e-139">A parameter definition in PowerShell Workflow runbooks has the following general form, where multiple parameters are separated by commas.</span></span>

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
> <span data-ttu-id="7b78e-140">När du definierar parametrar, om du inte anger den **obligatoriska** attribut, och sedan som standard parametern anses vara valfri.</span><span class="sxs-lookup"><span data-stu-id="7b78e-140">When you're defining parameters, if you don’t specify the **Mandatory** attribute, then by default, the parameter is considered optional.</span></span> <span data-ttu-id="7b78e-141">Även om du anger ett standardvärde för en parameter i PowerShell-arbetsflöde runbooks behandlas av PowerShell som en valfri parameter, oberoende av den **obligatoriska** attributvärdet.</span><span class="sxs-lookup"><span data-stu-id="7b78e-141">Also, if you set a default value for a parameter in PowerShell Workflow runbooks, it will be treated by PowerShell as an optional parameter, regardless of the **Mandatory** attribute value.</span></span>
> 
> 

<span data-ttu-id="7b78e-142">Exempelvis ska vi konfigurera indataparametrar för en PowerShell-arbetsflödesrunbook som matar ut information om virtuella datorer, en enda virtuell dator eller alla virtuella datorer inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7b78e-142">As an example, let’s configure the input parameters for a PowerShell Workflow runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="7b78e-143">Denna runbook innehåller två parametrar som visas i följande skärmbild: namnet på den virtuella datorn och namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7b78e-143">This runbook has two parameters as shown in the following screenshot: the name of virtual machine and the name of the resource group.</span></span>

![Automation PowerShell-arbetsflöde](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

<span data-ttu-id="7b78e-145">I den här parameterdefinition, parametrarna **$VMName** och **$resourceGroupName** är enkla parametrar av typen string.</span><span class="sxs-lookup"><span data-stu-id="7b78e-145">In this parameter definition, the parameters **$VMName** and **$resourceGroupName** are simple parameters of type string.</span></span> <span data-ttu-id="7b78e-146">PowerShell och PowerShell-arbetsflöde runbooks stöder dock alla enkla typer och komplexa typer som **objekt** eller **PSCredential** för indataparametrarna.</span><span class="sxs-lookup"><span data-stu-id="7b78e-146">However, PowerShell and PowerShell Workflow runbooks support all simple types and complex types, such as **object** or **PSCredential** for input parameters.</span></span>

<span data-ttu-id="7b78e-147">Om din runbook har ett objekt Indataparametern, använder du en PowerShell hash-tabell med (namn, värde) par att skicka in ett värde.</span><span class="sxs-lookup"><span data-stu-id="7b78e-147">If your runbook has an object type input parameter, then use a PowerShell hashtable with (name,value) pairs to pass in a value.</span></span> <span data-ttu-id="7b78e-148">Till exempel om du har följande parameter i en runbook:</span><span class="sxs-lookup"><span data-stu-id="7b78e-148">For example, if you have the following parameter in a runbook:</span></span>

     [Parameter (Mandatory = $true)]
     [object] $FullName

<span data-ttu-id="7b78e-149">Sedan kan du skicka följande värde i parametern:</span><span class="sxs-lookup"><span data-stu-id="7b78e-149">Then you can pass the following value to the parameter:</span></span>

    @{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}


## <a name="configure-input-parameters-in-graphical-runbooks"></a><span data-ttu-id="7b78e-150">Konfigurera indataparametrar i grafiska runbook-flöden</span><span class="sxs-lookup"><span data-stu-id="7b78e-150">Configure input parameters in graphical runbooks</span></span>
<span data-ttu-id="7b78e-151">Att [konfigurerar en grafisk runbook](automation-first-runbook-graphical.md) med indataparametrar, skapar vi en grafisk runbook som matar ut information om virtuella datorer, antingen en enskild virtuell dator eller alla virtuella datorer inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7b78e-151">To [configure a graphical runbook](automation-first-runbook-graphical.md) with input parameters, let’s create a graphical runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="7b78e-152">Konfigurera en runbook består av två viktiga aktiviteter som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="7b78e-152">Configuring a runbook consists of two major activities, as described below.</span></span>

<span data-ttu-id="7b78e-153">[**Autentisera Runbooks med Kör som-kontot Azure** ](automation-sec-configure-azure-runas-account.md) att autentisera med Azure.</span><span class="sxs-lookup"><span data-stu-id="7b78e-153">[**Authenticate Runbooks with Azure Run As account**](automation-sec-configure-azure-runas-account.md) to authenticate with Azure.</span></span>

<span data-ttu-id="7b78e-154">[**Get-AzureRmVm** ](https://msdn.microsoft.com/library/mt603718.aspx) att hämta egenskaperna för en virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7b78e-154">[**Get-AzureRmVm**](https://msdn.microsoft.com/library/mt603718.aspx) to get the properties of a virtual machines.</span></span>

<span data-ttu-id="7b78e-155">Du kan använda den [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) aktiviteten till utdata namnen på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7b78e-155">You can use the [**Write-Output**](https://technet.microsoft.com/library/hh849921.aspx) activity to output the names of virtual machines.</span></span> <span data-ttu-id="7b78e-156">Aktiviteten **Get-AzureRmVm** två parametrar i **virtuellt datornamn** och **resursgruppens namn**.</span><span class="sxs-lookup"><span data-stu-id="7b78e-156">The activity **Get-AzureRmVm** accepts two parameters, the **virtual machine name** and the **resource group name**.</span></span> <span data-ttu-id="7b78e-157">Eftersom de här parametrarna kan kräver olika värden varje gång du startar en runbook kan du lägga till indataparametrar till din runbook.</span><span class="sxs-lookup"><span data-stu-id="7b78e-157">Since these parameters could require different values each time you start the runbook, you can add input parameters to your runbook.</span></span> <span data-ttu-id="7b78e-158">Här följer stegen för att lägga till indataparametrar:</span><span class="sxs-lookup"><span data-stu-id="7b78e-158">Here are the steps to add input parameters:</span></span>

1. <span data-ttu-id="7b78e-159">Välj den grafiska runbooken från den **Runbooks** bladet och klicka sedan på [ **redigera** ](automation-graphical-authoring-intro.md) den.</span><span class="sxs-lookup"><span data-stu-id="7b78e-159">Select the graphical runbook from the **Runbooks** blade and then click [**Edit**](automation-graphical-authoring-intro.md) it.</span></span>
2. <span data-ttu-id="7b78e-160">Klicka på runbook-redigeraren **ingående och utgående** att öppna den **ingående och utgående** bladet.</span><span class="sxs-lookup"><span data-stu-id="7b78e-160">From the runbook editor, click **Input and output** to open the **Input and output** blade.</span></span>
   
    ![Grafisk Automation-runbook](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. <span data-ttu-id="7b78e-162">Den **ingående och utgående** bladet visar en lista över indataparametrar som definierats för runbooken.</span><span class="sxs-lookup"><span data-stu-id="7b78e-162">The **Input and output** blade displays a list of input parameters that are defined for the runbook.</span></span> <span data-ttu-id="7b78e-163">På det här bladet kan du lägga till en ny Indataparametern eller redigera konfigurationen av en befintlig indataparameter.</span><span class="sxs-lookup"><span data-stu-id="7b78e-163">On this blade, you can either add a new input parameter or edit the configuration of an existing input parameter.</span></span> <span data-ttu-id="7b78e-164">Om du vill lägga till en ny parameter för runbook, klickar du på **lägga till indata** att öppna den **runbookinmatningsparameter** bladet.</span><span class="sxs-lookup"><span data-stu-id="7b78e-164">To add a new parameter for the runbook, click **Add input** to open the **Runbook input parameter** blade.</span></span> <span data-ttu-id="7b78e-165">Där kan konfigurera du följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="7b78e-165">There, you can configure the following parameters:</span></span>
   
   | <span data-ttu-id="7b78e-166">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="7b78e-166">**Property**</span></span> | <span data-ttu-id="7b78e-167">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="7b78e-167">**Description**</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="7b78e-168">Namn</span><span class="sxs-lookup"><span data-stu-id="7b78e-168">Name</span></span> |<span data-ttu-id="7b78e-169">Krävs.</span><span class="sxs-lookup"><span data-stu-id="7b78e-169">Required.</span></span>  <span data-ttu-id="7b78e-170">Parameterns namn.</span><span class="sxs-lookup"><span data-stu-id="7b78e-170">The name of the parameter.</span></span> <span data-ttu-id="7b78e-171">Detta måste vara unika inom en runbook och kan bara innehålla bokstäver, siffror eller understreck.</span><span class="sxs-lookup"><span data-stu-id="7b78e-171">This must be unique within the runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="7b78e-172">Det måste börja med en bokstav.</span><span class="sxs-lookup"><span data-stu-id="7b78e-172">It must start with a letter.</span></span> |
   | <span data-ttu-id="7b78e-173">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7b78e-173">Description</span></span> |<span data-ttu-id="7b78e-174">Valfri.</span><span class="sxs-lookup"><span data-stu-id="7b78e-174">Optional.</span></span> <span data-ttu-id="7b78e-175">Beskrivning om syftet med indataparameter.</span><span class="sxs-lookup"><span data-stu-id="7b78e-175">Description about the purpose of input parameter.</span></span> |
   | <span data-ttu-id="7b78e-176">Typ</span><span class="sxs-lookup"><span data-stu-id="7b78e-176">Type</span></span> |<span data-ttu-id="7b78e-177">Valfri.</span><span class="sxs-lookup"><span data-stu-id="7b78e-177">Optional.</span></span> <span data-ttu-id="7b78e-178">Datatypen som förväntas för parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="7b78e-178">The data type that's expected for the parameter value.</span></span> <span data-ttu-id="7b78e-179">Parametrar som stöds är **sträng**, **Int32**, **Int64**, **Decimal**, **booleskt**,  **DateTime**, och **objektet**.</span><span class="sxs-lookup"><span data-stu-id="7b78e-179">Supported parameter types are **String**, **Int32**, **Int64**, **Decimal**, **Boolean**, **DateTime**, and **Object**.</span></span> <span data-ttu-id="7b78e-180">Om datatypen inte är markerat används som standard **sträng**.</span><span class="sxs-lookup"><span data-stu-id="7b78e-180">If a data type is not selected, it defaults to **String**.</span></span> |
   | <span data-ttu-id="7b78e-181">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="7b78e-181">Mandatory</span></span> |<span data-ttu-id="7b78e-182">Valfri.</span><span class="sxs-lookup"><span data-stu-id="7b78e-182">Optional.</span></span> <span data-ttu-id="7b78e-183">Anger om ett värde måste anges för parametern.</span><span class="sxs-lookup"><span data-stu-id="7b78e-183">Specifies whether a value must be provided for the parameter.</span></span> <span data-ttu-id="7b78e-184">Om du väljer **Ja**, och sedan ett värde måste anges när runbooken startar.</span><span class="sxs-lookup"><span data-stu-id="7b78e-184">If you choose **yes**, then a value must be provided when the runbook is started.</span></span> <span data-ttu-id="7b78e-185">Om du väljer **inga**, och sedan ett värde inte är nödvändiga när runbook startas och du kan endast ange ett standardvärde.</span><span class="sxs-lookup"><span data-stu-id="7b78e-185">If you choose **no**, then a value is not required when the runbook is started, and a default value may be set.</span></span> |
   | <span data-ttu-id="7b78e-186">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="7b78e-186">Default Value</span></span> |<span data-ttu-id="7b78e-187">Valfri.</span><span class="sxs-lookup"><span data-stu-id="7b78e-187">Optional.</span></span> <span data-ttu-id="7b78e-188">Anger ett värde som ska användas för parametern som ett värde inte skickas när runbook startas.</span><span class="sxs-lookup"><span data-stu-id="7b78e-188">Specifies a value that will be used for the parameter if a value is not passed in when the runbook is started.</span></span> <span data-ttu-id="7b78e-189">Ett standardvärde kan anges för en parameter som inte är obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="7b78e-189">A default value can be set for a parameter that's not mandatory.</span></span> <span data-ttu-id="7b78e-190">Om du vill ange ett standardvärde, Välj **anpassad**.</span><span class="sxs-lookup"><span data-stu-id="7b78e-190">To set a default value, choose **Custom**.</span></span> <span data-ttu-id="7b78e-191">Det här värdet används om inte ett annat värde anges när runbooken startas.</span><span class="sxs-lookup"><span data-stu-id="7b78e-191">This value is used unless another value is provided when the runbook is started.</span></span> <span data-ttu-id="7b78e-192">Välj **ingen** om du inte vill ange någon standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="7b78e-192">Choose **None** if you don’t want to provide any default value.</span></span> |
   
    ![Lägga till nya indata](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. <span data-ttu-id="7b78e-194">Skapa två parametrar med följande egenskaper kommer att användas av den **Get-AzureRmVm** aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="7b78e-194">Create two parameters with the following properties that will be used by the **Get-AzureRmVm** activity:</span></span>
   
   * <span data-ttu-id="7b78e-195">**Parameter1:**</span><span class="sxs-lookup"><span data-stu-id="7b78e-195">**Parameter1:**</span></span>
     
     * <span data-ttu-id="7b78e-196">Namn - VMName</span><span class="sxs-lookup"><span data-stu-id="7b78e-196">Name - VMName</span></span>
     * <span data-ttu-id="7b78e-197">Typ - sträng</span><span class="sxs-lookup"><span data-stu-id="7b78e-197">Type - String</span></span>
     * <span data-ttu-id="7b78e-198">Obligatoriska - Nej</span><span class="sxs-lookup"><span data-stu-id="7b78e-198">Mandatory - No</span></span>
   * <span data-ttu-id="7b78e-199">**Parameter2:**</span><span class="sxs-lookup"><span data-stu-id="7b78e-199">**Parameter2:**</span></span>
     
     * <span data-ttu-id="7b78e-200">Namn - resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="7b78e-200">Name - resourceGroupName</span></span>
     * <span data-ttu-id="7b78e-201">Typ - sträng</span><span class="sxs-lookup"><span data-stu-id="7b78e-201">Type - String</span></span>
     * <span data-ttu-id="7b78e-202">Obligatoriska - Nej</span><span class="sxs-lookup"><span data-stu-id="7b78e-202">Mandatory - No</span></span>
     * <span data-ttu-id="7b78e-203">Standardvärde - anpassad</span><span class="sxs-lookup"><span data-stu-id="7b78e-203">Default value - Custom</span></span>
     * <span data-ttu-id="7b78e-204">Anpassade standardvärde - \<namnet på resursgruppen som innehåller de virtuella datorerna ></span><span class="sxs-lookup"><span data-stu-id="7b78e-204">Custom default value - \<Name of the resource group that contains the virtual machines></span></span>
5. <span data-ttu-id="7b78e-205">När du lägger till parametrarna, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7b78e-205">Once you add the parameters, click **OK**.</span></span>  <span data-ttu-id="7b78e-206">Nu kan du visa dem i den **indata och utdata bladet**.</span><span class="sxs-lookup"><span data-stu-id="7b78e-206">You can now view them in the **Input and output blade**.</span></span> <span data-ttu-id="7b78e-207">Klicka på **OK** igen, och klicka sedan på **spara** och **publicera** din runbook.</span><span class="sxs-lookup"><span data-stu-id="7b78e-207">Click **OK** again, and then click **Save** and **Publish** your runbook.</span></span>

## <a name="assign-values-to-input-parameters-in-runbooks"></a><span data-ttu-id="7b78e-208">Tilldela värden till indataparametrar i runbooks</span><span class="sxs-lookup"><span data-stu-id="7b78e-208">Assign values to input parameters in runbooks</span></span>
<span data-ttu-id="7b78e-209">Du kan skicka värden för att ange parametrar i runbooks i följande scenarier.</span><span class="sxs-lookup"><span data-stu-id="7b78e-209">You can pass values to input parameters in runbooks in the following scenarios.</span></span>

### <a name="start-a-runbook-and-assign-parameters"></a><span data-ttu-id="7b78e-210">Starta en runbook och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="7b78e-210">Start a runbook and assign parameters</span></span>
<span data-ttu-id="7b78e-211">En runbook kan startas på många olika sätt: genom Azure-portalen med en webhook, med PowerShell-cmdletar, med REST API eller med SDK.</span><span class="sxs-lookup"><span data-stu-id="7b78e-211">A runbook can be started many ways: through the Azure portal, with a webhook, with PowerShell cmdlets, with the REST API, or with the SDK.</span></span> <span data-ttu-id="7b78e-212">Nedan diskuterar vi olika metoder för att starta en runbook och tilldela parametrar.</span><span class="sxs-lookup"><span data-stu-id="7b78e-212">Below we discuss different methods for starting a runbook and assigning parameters.</span></span>

#### <a name="start-a-published-runbook-by-using-the-azure-portal-and-assign-parameters"></a><span data-ttu-id="7b78e-213">Starta en publicerad runbook med hjälp av Azure portal och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="7b78e-213">Start a published runbook by using the Azure portal and assign parameters</span></span>
<span data-ttu-id="7b78e-214">När du [starta runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), **starta Runbook** blad öppnas och du kan konfigurera värden för parametrarna som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="7b78e-214">When you [start the runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), the **Start Runbook** blade opens and you can configure values for the parameters that you just created.</span></span>

![Börja använda portalen](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

<span data-ttu-id="7b78e-216">Du kan se de attribut som har angetts för parametern i etiketten under rutan indata.</span><span class="sxs-lookup"><span data-stu-id="7b78e-216">In the label beneath the input box, you can see the attributes that have been set for the parameter.</span></span> <span data-ttu-id="7b78e-217">Attribut är obligatoriska eller valfria, typ och standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="7b78e-217">Attributes include mandatory or optional, type, and  default value.</span></span> <span data-ttu-id="7b78e-218">Du kan se alla viktig information som du behöver fatta beslut om parametern indatavärden i Hjälp-pratbubblor bredvid namnet på parametern.</span><span class="sxs-lookup"><span data-stu-id="7b78e-218">In the help balloon next to the parameter name, you can see all the key information you need to make decisions about parameter input values.</span></span> <span data-ttu-id="7b78e-219">Informationen omfattar om en parameter är obligatoriska eller valfria.</span><span class="sxs-lookup"><span data-stu-id="7b78e-219">This information includes whether a parameter is mandatory or optional.</span></span> <span data-ttu-id="7b78e-220">Även typ och standardvärdet (eventuella) och annan användbar information.</span><span class="sxs-lookup"><span data-stu-id="7b78e-220">It also includes the type and default value (if any), and other helpful notes.</span></span>

![Hjälp pratbubblor](media/automation-runbook-input-parameters/automation-05-helpbaloon.png)

> [!NOTE]
> <span data-ttu-id="7b78e-222">Stöd för parametrar av typen String **tom** String värden.</span><span class="sxs-lookup"><span data-stu-id="7b78e-222">String type parameters support **Empty** String values.</span></span>  <span data-ttu-id="7b78e-223">Ange **[EmptyString]** i Indataparametern kommer rutan att skicka en tom sträng i parametern.</span><span class="sxs-lookup"><span data-stu-id="7b78e-223">Entering **[EmptyString]** in the input parameter box will pass an empty string to the parameter.</span></span> <span data-ttu-id="7b78e-224">Dessutom stöder inte typen strängparametrar **Null** värden som skickas.</span><span class="sxs-lookup"><span data-stu-id="7b78e-224">Also, String type parameters don’t support **Null** values being passed.</span></span> <span data-ttu-id="7b78e-225">Om du inte skickar ett värde till parametern sträng, sedan tolkas PowerShell det som null.</span><span class="sxs-lookup"><span data-stu-id="7b78e-225">If you don’t pass any value to the String parameter, then PowerShell will interpret it as null.</span></span>
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a><span data-ttu-id="7b78e-226">Starta en publicerad runbook med hjälp av PowerShell-cmdlets och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="7b78e-226">Start a published runbook by using PowerShell cmdlets and assign parameters</span></span>
* <span data-ttu-id="7b78e-227">**Azure Resource Manager-cmdlets:** du kan starta en Automation-runbook som har skapats i en resursgrupp med hjälp av [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span><span class="sxs-lookup"><span data-stu-id="7b78e-227">**Azure Resource Manager cmdlets:** You can start an Automation runbook that was created in a resource group by using [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span></span>
  
  <span data-ttu-id="7b78e-228">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="7b78e-228">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* <span data-ttu-id="7b78e-229">**Azure Service Management-cmdlets:** du kan starta en automation-runbook som har skapats i en standard-resursgrupp med hjälp av [Start AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span><span class="sxs-lookup"><span data-stu-id="7b78e-229">**Azure Service Management cmdlets:** You can start an automation runbook that was created in a default resource group by using [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span></span>
  
  <span data-ttu-id="7b78e-230">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="7b78e-230">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> <span data-ttu-id="7b78e-231">När du startar en runbook med hjälp av PowerShell-cmdletar, en standardparameter **MicrosoftApplicationManagementStartedBy** skapas med värdet **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="7b78e-231">When you start a runbook by using PowerShell cmdlets, a default parameter, **MicrosoftApplicationManagementStartedBy** is created with the value **PowerShell**.</span></span> <span data-ttu-id="7b78e-232">Du kan visa den här parametern i den **Jobbdetaljer** bladet.</span><span class="sxs-lookup"><span data-stu-id="7b78e-232">You can view this parameter in the **Job details** blade.</span></span>  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a><span data-ttu-id="7b78e-233">Starta en runbook med hjälp av ett SDK och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="7b78e-233">Start a runbook by using an SDK and assign parameters</span></span>
* <span data-ttu-id="7b78e-234">**Azure Resource Manager-metod:** du kan starta en runbook med hjälp av SDK för ett programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="7b78e-234">**Azure Resource Manager method:** You can start a runbook by using the SDK of a programming language.</span></span> <span data-ttu-id="7b78e-235">Nedan visas ett C# kodfragment för att starta en runbook i Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="7b78e-235">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="7b78e-236">Du kan visa all kod på vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="7b78e-236">You can view all the code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>  
  
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
* <span data-ttu-id="7b78e-237">**Azure Service Management-metod:** du kan starta en runbook med hjälp av SDK för ett programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="7b78e-237">**Azure Service Management method:** You can start a runbook by using the SDK of a programming language.</span></span> <span data-ttu-id="7b78e-238">Nedan visas ett C# kodfragment för att starta en runbook i Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="7b78e-238">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="7b78e-239">Du kan visa all kod på vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="7b78e-239">You can view all the code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>
  
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
  
  <span data-ttu-id="7b78e-240">Skapa en ordlista för att lagra runbook-parametrar för att starta den här metoden **VMName** och **resourceGroupName**, och deras värden.</span><span class="sxs-lookup"><span data-stu-id="7b78e-240">To start this method, create a dictionary to store the runbook parameters, **VMName** and  **resourceGroupName**, and their values.</span></span> <span data-ttu-id="7b78e-241">Starta runbook.</span><span class="sxs-lookup"><span data-stu-id="7b78e-241">Then start the runbook.</span></span> <span data-ttu-id="7b78e-242">Nedan visas kodfragmentet C# för att anropa metoden som har definierats ovan.</span><span class="sxs-lookup"><span data-stu-id="7b78e-242">Below is the C# code snippet for calling the method that's defined above.</span></span>
  
  ```
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters to the dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call the StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-the-rest-api-and-assign-parameters"></a><span data-ttu-id="7b78e-243">Starta en runbook med hjälp av REST-API och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="7b78e-243">Start a runbook by using the REST API and assign parameters</span></span>
<span data-ttu-id="7b78e-244">Ett runbook-jobb kan skapas och igång med Azure Automation REST-API med hjälp av den **PLACERA** metod med följande URI-begäran.</span><span class="sxs-lookup"><span data-stu-id="7b78e-244">A runbook job can be created and started with the Azure Automation REST API by using the **PUT** method with the following request URI.</span></span>

    https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`

<span data-ttu-id="7b78e-245">I URI-begäran, ersätter du följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="7b78e-245">In the request URI, replace the following parameters:</span></span>

* <span data-ttu-id="7b78e-246">**prenumerations-id:** ditt Azure-prenumeration-ID.</span><span class="sxs-lookup"><span data-stu-id="7b78e-246">**subscription-id:** Your Azure subscription ID.</span></span>  
* <span data-ttu-id="7b78e-247">**molntjänstnamnet-:** namnet på molnet tjänsten som begäran ska skickas.</span><span class="sxs-lookup"><span data-stu-id="7b78e-247">**cloud-service-name:** The name of the cloud service to which the request should be sent.</span></span>  
* <span data-ttu-id="7b78e-248">**Automation-kontonamn:** namnet på ditt automation-konto som ligger inom den angivna Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="7b78e-248">**automation-account-name:** The name of your automation account that's hosted within the specified cloud service.</span></span>  
* <span data-ttu-id="7b78e-249">**jobb-id:** GUID för jobbet.</span><span class="sxs-lookup"><span data-stu-id="7b78e-249">**job-id:** The GUID for the job.</span></span> <span data-ttu-id="7b78e-250">GUID i PowerShell kan skapas med hjälp av den **[GUID]::NewGuid(). ToString()** kommando.</span><span class="sxs-lookup"><span data-stu-id="7b78e-250">GUIDs in PowerShell can be created by using the **[GUID]::NewGuid().ToString()** command.</span></span>

<span data-ttu-id="7b78e-251">Använd begärandetexten för att skicka parametrar till runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="7b78e-251">In order to pass parameters to the runbook job, use the request body.</span></span> <span data-ttu-id="7b78e-252">Det tar följande två egenskaper i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="7b78e-252">It takes the following two properties provided in JSON format:</span></span>

* <span data-ttu-id="7b78e-253">**Runbook-namn:** krävs.</span><span class="sxs-lookup"><span data-stu-id="7b78e-253">**Runbook name:** Required.</span></span> <span data-ttu-id="7b78e-254">Namnet på runbook för att jobbet ska starta.</span><span class="sxs-lookup"><span data-stu-id="7b78e-254">The name of the runbook for the job to start.</span></span>  
* <span data-ttu-id="7b78e-255">**Runbook-parametrar:** valfritt.</span><span class="sxs-lookup"><span data-stu-id="7b78e-255">**Runbook parameters:** Optional.</span></span> <span data-ttu-id="7b78e-256">En ordlista med parameterlistan i (namn, värde) format där namnet ska vara av typen sträng och värdet kan vara ett giltigt JSON-värde.</span><span class="sxs-lookup"><span data-stu-id="7b78e-256">A dictionary of the parameter list in (name, value) format where name should be of String type and value can be any valid JSON value.</span></span>

<span data-ttu-id="7b78e-257">Om du vill starta den **Get-AzureVMTextual** runbook som har skapats tidigare med **VMName** och **resourceGroupName** som parametrar, använda följande JSON-format för begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="7b78e-257">If you want to start the **Get-AzureVMTextual** runbook that was created earlier with **VMName** and **resourceGroupName** as parameters, use the following JSON format for the request body.</span></span>

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

<span data-ttu-id="7b78e-258">En HTTP-statuskod 201 returneras om jobbet har skapats.</span><span class="sxs-lookup"><span data-stu-id="7b78e-258">A HTTP status code 201 is returned if the job is successfully created.</span></span> <span data-ttu-id="7b78e-259">Mer information om svarshuvuden och svarstexten finns i artikel om hur du [skapa ett runbook-jobb med hjälp av REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span><span class="sxs-lookup"><span data-stu-id="7b78e-259">For more information on response headers and the response body, refer to the article about how to [create a runbook job by using the REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span></span>

### <a name="test-a-runbook-and-assign-parameters"></a><span data-ttu-id="7b78e-260">Testa en runbook och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="7b78e-260">Test a runbook and assign parameters</span></span>
<span data-ttu-id="7b78e-261">När du [testa utkastversionen för din runbook](automation-testing-runbook.md) med hjälp av alternativet testa den **testa** blad öppnas och du kan konfigurera värden för parametrarna som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="7b78e-261">When you [test the draft version of your runbook](automation-testing-runbook.md) by using the test option, the **Test** blade opens and you can configure values for the parameters that you just created.</span></span>

![Testa och tilldela parametrar](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-to-a-runbook-and-assign-parameters"></a><span data-ttu-id="7b78e-263">Länka ett schema till en runbook och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="7b78e-263">Link a schedule to a runbook and assign parameters</span></span>
<span data-ttu-id="7b78e-264">Du kan [länka ett schema](automation-schedules.md) till din runbook så att runbook startar vid en viss tid.</span><span class="sxs-lookup"><span data-stu-id="7b78e-264">You can [link a schedule](automation-schedules.md) to your runbook so that the runbook starts at a specific time.</span></span> <span data-ttu-id="7b78e-265">Du kan tilldela indataparametrar när du skapar schemat och runbook kommer att använda dessa värden när den har startats med schemat.</span><span class="sxs-lookup"><span data-stu-id="7b78e-265">You assign input parameters when you create the schedule, and the runbook will use these values when it is started by the schedule.</span></span> <span data-ttu-id="7b78e-266">Du kan inte spara schemat förrän alla obligatoriska parametervärden har angetts.</span><span class="sxs-lookup"><span data-stu-id="7b78e-266">You can’t save the schedule until all mandatory parameter values are provided.</span></span>

![Schemalägga och tilldela parametrar](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a><span data-ttu-id="7b78e-268">Skapa en webhook för en runbook och tilldela parametrar</span><span class="sxs-lookup"><span data-stu-id="7b78e-268">Create a webhook for a runbook and assign parameters</span></span>
<span data-ttu-id="7b78e-269">Du kan skapa en [webhook](automation-webhooks.md) för din runbook och konfigurera indataparametrarna för runbook.</span><span class="sxs-lookup"><span data-stu-id="7b78e-269">You can create a [webhook](automation-webhooks.md) for your runbook and configure runbook input parameters.</span></span> <span data-ttu-id="7b78e-270">Du kan inte spara webhooken förrän alla obligatoriska parametervärden har angetts.</span><span class="sxs-lookup"><span data-stu-id="7b78e-270">You can’t save the webhook until all mandatory parameter values are provided.</span></span>

![Skapa webhook och tilldela parametrar](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

<span data-ttu-id="7b78e-272">När du kör en runbook med hjälp av en webhook fördefinierade Indataparametern  **[Webhookdata](automation-webhooks.md#details-of-a-webhook)**  skickas tillsammans med indataparametrar som du har definierat.</span><span class="sxs-lookup"><span data-stu-id="7b78e-272">When you execute a runbook by using a webhook, the predefined input parameter **[Webhookdata](automation-webhooks.md#details-of-a-webhook)** is sent, along with the input parameters that you defined.</span></span> <span data-ttu-id="7b78e-273">Du kan klicka på för att expandera den **WebhookData** parameter för mer information.</span><span class="sxs-lookup"><span data-stu-id="7b78e-273">You can click to expand the **WebhookData** parameter for more details.</span></span>

![WebhookData-parameter](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a><span data-ttu-id="7b78e-275">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7b78e-275">Next steps</span></span>
* <span data-ttu-id="7b78e-276">Mer information om runbook ingående och utgående finns [Azure Automation: runbook indata, utdata och kapslade runbooks](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span><span class="sxs-lookup"><span data-stu-id="7b78e-276">For more information on runbook input and output, see [Azure Automation: runbook input, output, and nested runbooks](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span></span>
* <span data-ttu-id="7b78e-277">Mer information om olika sätt att starta en runbook finns [starta en runbook](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="7b78e-277">For details about different ways to start a runbook, see [Starting a runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="7b78e-278">Om du vill redigera en textrepresentation runbook, referera till [redigera textrepresentation runbooks](automation-edit-textual-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="7b78e-278">To edit a textual runbook, refer to [Editing textual runbooks](automation-edit-textual-runbook.md).</span></span>
* <span data-ttu-id="7b78e-279">Om du vill redigera en grafisk runbook avser [grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="7b78e-279">To edit a graphical runbook, refer to [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md).</span></span>

