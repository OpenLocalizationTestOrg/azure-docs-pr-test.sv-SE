---
title: "aaaVariable tillgångar i Azure Automation | Microsoft Docs"
description: "Variabeln tillgångar är värden som är tillgängliga tooall runbooks och i Azure Automation DSC-konfigurationer.  Den här artikeln förklarar hello information om variabler och hur toowork med dem i både text och grafiska redigering."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: b880c15f-46f5-4881-8e98-e034cc5a66ec
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/09/2017
ms.author: magoedte;bwren
ms.openlocfilehash: f9daa49fc1dc883ffb218a9adf26e36df1d6bb27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="variable-assets-in-azure-automation"></a><span data-ttu-id="a5ebc-104">Variabeln tillgångar i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="a5ebc-104">Variable assets in Azure Automation</span></span>

<span data-ttu-id="a5ebc-105">Variabeln tillgångar är värden som är tillgängliga tooall runbooks och DSC-konfigurationer i automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-105">Variable assets are values that are available tooall runbooks and DSC configurations in your automation account.</span></span> <span data-ttu-id="a5ebc-106">De kan skapas, ändras och hämtas från hello Azure-portalen Windows PowerShell och inifrån en runbook eller DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-106">They can be created, modified, and retrieved from hello Azure portal, Windows PowerShell, and from within a runbook or DSC configuration.</span></span> <span data-ttu-id="a5ebc-107">Automationsvariabler är användbara för hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="a5ebc-107">Automation variables are useful for hello following scenarios:</span></span>

- <span data-ttu-id="a5ebc-108">Dela ett värde mellan flera runbooks eller DSC-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-108">Share a value between multiple runbooks or DSC configurations.</span></span>

- <span data-ttu-id="a5ebc-109">Dela ett värde mellan flera jobb från hello samma runbook eller DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-109">Share a value between multiple jobs from hello same runbook or DSC configuration.</span></span>

- <span data-ttu-id="a5ebc-110">Hantera ett värde från hello-portalen eller hello Windows PowerShell-kommandoraden som används av runbooks eller DSC-konfigurationer, till exempel en uppsättning gemensamma konfigurationsobjekt som specifik lista av VM-namn, en specifik resursgrupp, en AD-domännamn och så vidare.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-110">Manage a value from hello portal or from hello Windows PowerShell command line that is used by runbooks or DSC configurations, such as a set of common configuration items like specific list of VM names, a specific resource group,  an AD domain name, etc.</span></span>  

<span data-ttu-id="a5ebc-111">Automationsvariabler är beständiga så att de fortsätter toobe tillgängliga även om hello runbook eller DSC-konfigurationen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-111">Automation variables are persisted so that they continue toobe available even if hello runbook or DSC configuration fails.</span></span>  <span data-ttu-id="a5ebc-112">Detta kan även ett värde toobe som angetts av en runbook som sedan används av en annan eller används av hello samma runbook eller DSC-konfiguration hello nästa gång den körs.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-112">This also allows a value toobe set by one runbook that is then used by another, or is used by hello same runbook or DSC configuration hello next time that it is run.</span></span>

<span data-ttu-id="a5ebc-113">När en variabel har skapats kan du ange att den lagras krypterade.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-113">When a variable is created, you can specify that it be stored encrypted.</span></span>  <span data-ttu-id="a5ebc-114">När en variabel krypteras, lagras den säkert i Azure Automation och dess värde kan inte hämtas från hello [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) cmdlet som levereras som en del av hello Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-114">When a variable is encrypted, it is stored securely in Azure Automation, and its value cannot be retrieved from hello [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) cmdlet that ships as part of hello Azure PowerShell module.</span></span>  <span data-ttu-id="a5ebc-115">hello enda sättet att ett krypterat värde kan hämtas är från hello **Get-automationvariable,** aktivitet i en runbook eller DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-115">hello only way that an encrypted value can be retrieved is from hello **Get-AutomationVariable** activity in a runbook or DSC configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="a5ebc-116">Säkra tillgångar i Azure Automation inkluderar autentiseringsuppgifter, certifikat, anslutningar och krypterade variabler.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-116">Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables.</span></span> <span data-ttu-id="a5ebc-117">Dessa tillgångar krypteras och lagras i hello Azure Automation med en unik nyckel som skapas för varje automation-konto.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-117">These assets are encrypted and stored in hello Azure Automation using a unique key that is generated for each automation account.</span></span> <span data-ttu-id="a5ebc-118">Den här nyckeln är krypterad med ett certifikat för master och lagras i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-118">This key is encrypted by a master certificate and stored in Azure Automation.</span></span> <span data-ttu-id="a5ebc-119">Innan de lagras en säker tillgång hello nyckel för hello automation-konto dekrypteras med hello master certifikatet och sedan används tooencrypt hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-119">Before storing a secure asset, hello key for hello automation account is decrypted using hello master certificate and then used tooencrypt hello asset.</span></span>

## <a name="variable-types"></a><span data-ttu-id="a5ebc-120">Datatyper</span><span class="sxs-lookup"><span data-stu-id="a5ebc-120">Variable types</span></span>

<span data-ttu-id="a5ebc-121">När du skapar en variabel med hello Azure-portalen, måste du ange en datatyp från listrutan hello så hello portal kan visa hello lämplig kontroll för att ange hello variabelvärdet.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-121">When you create a variable with hello Azure portal, you must specify a data type from hello drop-down list so hello portal can display hello appropriate control for entering hello variable value.</span></span> <span data-ttu-id="a5ebc-122">hello variabel är inte begränsade toothis data typ, men du måste ange hello variabel med Windows PowerShell om du vill toospecify ett värde av en annan typ.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-122">hello variable is not restricted toothis data type, but you must set hello variable using Windows PowerShell if you want toospecify a value of a different type.</span></span> <span data-ttu-id="a5ebc-123">Om du anger **inte har definierats**, och sedan hello hello variabels värdet för**$null**, och du måste ange hello-värde med hello [Set AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) cmdlet eller **Set-automationvariable,** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-123">If you specify **Not defined**, then hello value of hello variable will be set too**$null**, and you must set hello value with hello [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) cmdlet or **Set-AutomationVariable** activity.</span></span>  <span data-ttu-id="a5ebc-124">Du kan inte skapa eller ändra hello värdet för en variabel för komplex typ i hello-portalen, men du kan ange ett värde av valfri typ med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-124">You cannot create or change hello value for a complex variable type in hello portal, but you can provide a value of any type using Windows PowerShell.</span></span> <span data-ttu-id="a5ebc-125">Komplexa typer returneras som en [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).</span><span class="sxs-lookup"><span data-stu-id="a5ebc-125">Complex types will be returned as a [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).</span></span>

<span data-ttu-id="a5ebc-126">Du kan lagra flera värden tooa enda variabel genom att skapa en matris eller hash-tabell och spara den toohello variabeln.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-126">You can store multiple values tooa single variable by creating an array or hashtable and saving it toohello variable.</span></span>

<span data-ttu-id="a5ebc-127">hello nedan följer en lista över variabla typer som är tillgängliga i Automation:</span><span class="sxs-lookup"><span data-stu-id="a5ebc-127">hello following are a list of variable types available in Automation:</span></span>

* <span data-ttu-id="a5ebc-128">Sträng</span><span class="sxs-lookup"><span data-stu-id="a5ebc-128">String</span></span>
* <span data-ttu-id="a5ebc-129">Integer</span><span class="sxs-lookup"><span data-stu-id="a5ebc-129">Integer</span></span>
* <span data-ttu-id="a5ebc-130">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="a5ebc-130">DateTime</span></span>
* <span data-ttu-id="a5ebc-131">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="a5ebc-131">Boolean</span></span>
* <span data-ttu-id="a5ebc-132">Null</span><span class="sxs-lookup"><span data-stu-id="a5ebc-132">Null</span></span>

## <a name="cmdlets-and-workflow-activities"></a><span data-ttu-id="a5ebc-133">Aktiviteter-cmdlets och arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="a5ebc-133">Cmdlets and workflow activities</span></span>

<span data-ttu-id="a5ebc-134">hello-cmdlets i följande tabell hello är används toocreate och hantera automatisering variabler med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-134">hello cmdlets in hello following table are used toocreate and manage Automation variables with Windows PowerShell.</span></span> <span data-ttu-id="a5ebc-135">De levereras som en del av hello [Azure PowerShell-modulen](../powershell-install-configure.md) som är tillgänglig för användning i Automation-runbooks och DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-135">They ship as part of hello [Azure PowerShell module](../powershell-install-configure.md) which is available for use in Automation runbooks and DSC configuration.</span></span>

|<span data-ttu-id="a5ebc-136">Cmdlet: ar</span><span class="sxs-lookup"><span data-stu-id="a5ebc-136">Cmdlets</span></span>|<span data-ttu-id="a5ebc-137">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a5ebc-137">Description</span></span>|
|:---|:---|
|[<span data-ttu-id="a5ebc-138">Get-AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="a5ebc-138">Get-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603849.aspx)|<span data-ttu-id="a5ebc-139">Hämtar hello-värdet för en befintlig variabel.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-139">Retrieves hello value of an existing variable.</span></span>|
|[<span data-ttu-id="a5ebc-140">Ny AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="a5ebc-140">New-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603613.aspx)|<span data-ttu-id="a5ebc-141">Skapar en ny variabel och anger dess värde.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-141">Creates a new variable and sets its value.</span></span>|
|[<span data-ttu-id="a5ebc-142">Ta bort AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="a5ebc-142">Remove-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt619354.aspx)|<span data-ttu-id="a5ebc-143">Tar bort en befintlig variabel.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-143">Removes an existing variable.</span></span>|
|[<span data-ttu-id="a5ebc-144">Ange AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="a5ebc-144">Set-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603601.aspx)|<span data-ttu-id="a5ebc-145">Anger hello-värde för en befintlig variabel.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-145">Sets hello value for an existing variable.</span></span>|

<span data-ttu-id="a5ebc-146">hello-arbetsflödesaktiviteter i hello i den följande tabellen finns används tooaccess automationsvariabler i en runbook.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-146">hello workflow activities in hello following table are used tooaccess Automation variables in a runbook.</span></span> <span data-ttu-id="a5ebc-147">De är bara tillgängliga för användning i en runbook eller DSC-konfigurationen och levereras inte som en del av hello Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-147">They are only available for use in a runbook or DSC configuration, and do not ship as part of hello Azure PowerShell module.</span></span>

|<span data-ttu-id="a5ebc-148">Arbetsflödesaktiviteter</span><span class="sxs-lookup"><span data-stu-id="a5ebc-148">Workflow Activities</span></span>|<span data-ttu-id="a5ebc-149">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a5ebc-149">Description</span></span>|
|:---|:---|
|<span data-ttu-id="a5ebc-150">Get-automationvariable</span><span class="sxs-lookup"><span data-stu-id="a5ebc-150">Get-AutomationVariable</span></span>|<span data-ttu-id="a5ebc-151">Hämtar hello-värdet för en befintlig variabel.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-151">Retrieves hello value of an existing variable.</span></span>|
|<span data-ttu-id="a5ebc-152">Set-automationvariable</span><span class="sxs-lookup"><span data-stu-id="a5ebc-152">Set-AutomationVariable</span></span>|<span data-ttu-id="a5ebc-153">Anger hello-värde för en befintlig variabel.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-153">Sets hello value for an existing variable.</span></span>|

> [!NOTE] 
> <span data-ttu-id="a5ebc-154">Du bör undvika att använda variabler i hello – Name-parametern i **Get-automationvariable,** i en runbook eller DSC-konfigurationen eftersom detta kan göra det svårare att hitta beroenden mellan runbooks eller DSC-konfiguration och automatisering variabler i designläge.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-154">You should avoid using variables in hello –Name parameter of **Get-AutomationVariable**  in a runbook or DSC configuration since this can complicate discovering dependencies between runbooks or DSC configuration, and Automation variables at design time.</span></span>

## <a name="creating-a-new-automation-variable"></a><span data-ttu-id="a5ebc-155">Skapa en ny Automation-variabel</span><span class="sxs-lookup"><span data-stu-id="a5ebc-155">Creating a new Automation variable</span></span>

### <a name="toocreate-a-new-variable-with-hello-azure-portal"></a><span data-ttu-id="a5ebc-156">toocreate en ny variabel med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a5ebc-156">toocreate a new variable with hello Azure portal</span></span>

1. <span data-ttu-id="a5ebc-157">Från ditt Automation-konto klickar du på hello **tillgångar** panelen och klicka sedan på hello **tillgångar** bladet väljer **variabler**.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-157">From your Automation account, click hello **Assets** tile and then on hello **Assets** blade, select **Variables**.</span></span>
2. <span data-ttu-id="a5ebc-158">På hello **variabler** panelen, väljer **lägga till en variabel**.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-158">On hello **Variables** tile, select **Add a variable**.</span></span>
3. <span data-ttu-id="a5ebc-159">Slutföra hello alternativ på hello **ny variabel** bladet och klicka på **skapa** spara hello ny variabel.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-159">Complete hello options on hello **New Variable** blade and click **Create** save hello new variable.</span></span>

### <a name="toocreate-a-new-variable-with-windows-powershell"></a><span data-ttu-id="a5ebc-160">toocreate en ny variabel med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="a5ebc-160">toocreate a new variable with Windows PowerShell</span></span>

<span data-ttu-id="a5ebc-161">Hej [ny AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) cmdlet skapar en ny variabel och anger sitt ursprungliga värde.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-161">hello [New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) cmdlet creates a new variable and sets its initial value.</span></span> <span data-ttu-id="a5ebc-162">Du kan hämta hello värde med hjälp av [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx).</span><span class="sxs-lookup"><span data-stu-id="a5ebc-162">You can retrieve hello value using [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx).</span></span> <span data-ttu-id="a5ebc-163">Om hello-värdet är en enkel typ, returneras samma typ..</span><span class="sxs-lookup"><span data-stu-id="a5ebc-163">If hello value is a simple type, then that same type is returned.</span></span> <span data-ttu-id="a5ebc-164">Om det är en komplex typ och sedan en **PSCustomObject** returneras.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-164">If it’s a complex type, then a **PSCustomObject** is returned.</span></span>

<span data-ttu-id="a5ebc-165">hello följande exempel kommandon visar hur toocreate en variabel av typen string och returnerar sedan värdet.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-165">hello following sample commands show how toocreate a variable of type string and then return its value.</span></span>

    New-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

<span data-ttu-id="a5ebc-166">hello följande exempel kommandon visar hur toocreate en variabel med en komplex och returnerar sedan dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-166">hello following sample commands show how toocreate a variable with a complex type and then return its properties.</span></span> <span data-ttu-id="a5ebc-167">I det här fallet en virtuell dator objekt från **Get-AzureRmVm** används.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-167">In this case, a virtual machine object from **Get-AzureRmVm** is used.</span></span>

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a><span data-ttu-id="a5ebc-168">Använda en variabel i en runbook eller DSC-konfiguration</span><span class="sxs-lookup"><span data-stu-id="a5ebc-168">Using a variable in a runbook or DSC configuration</span></span>

<span data-ttu-id="a5ebc-169">Använd hello **Set-automationvariable,** aktiviteten tooset hello värdet för en Automation-variabel i en runbook eller DSC-konfigurationen och hello **Get-automationvariable,** tooretrieve den.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-169">Use hello **Set-AutomationVariable** activity tooset hello value of an Automation variable in a runbook or DSC configuration, and hello **Get-AutomationVariable** tooretrieve it.</span></span>  <span data-ttu-id="a5ebc-170">Du bör inte använda hello **Set AzureAutomationVariable** eller **Get-AzureAutomationVariable** cmdletarna i en runbook eller DSC-konfigurationen eftersom de är mindre effektivt än hello arbetsflödesaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-170">You shouldn't use hello **Set-AzureAutomationVariable** or  **Get-AzureAutomationVariable** cmdlets in a runbook or DSC configuration since they are less efficient than hello workflow activities.</span></span>  <span data-ttu-id="a5ebc-171">Du kan också hämta hello värdet för säker variabler med **Get-AzureAutomationVariable**.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-171">You also cannot retrieve hello value of secure variables with **Get-AzureAutomationVariable**.</span></span>  <span data-ttu-id="a5ebc-172">Hej endast sätt toocreate en ny variabel från inifrån en runbook eller DSC-konfigurationen är toouse hello [ny AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-172">hello only way toocreate a new variable from within a runbook or DSC configuration is toouse hello [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx)  cmdlet.</span></span>


### <a name="textual-runbook-samples"></a><span data-ttu-id="a5ebc-173">Textrepresentation runbook-exempel</span><span class="sxs-lookup"><span data-stu-id="a5ebc-173">Textual runbook samples</span></span>

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a><span data-ttu-id="a5ebc-174">Ställa in och hämta ett enkelt värde från en variabel</span><span class="sxs-lookup"><span data-stu-id="a5ebc-174">Setting and retrieving a simple value from a variable</span></span>

<span data-ttu-id="a5ebc-175">hello följande exempel kommandon visar hur tooset och hämtar en variabel i text-runbook.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-175">hello following sample commands show how tooset and retrieve a variable in a textual runbook.</span></span> <span data-ttu-id="a5ebc-176">I det här exemplet förutsätts att variabler av typen heltal med namnet *NumberOfIterations* och *NumberOfRunnings* och en variabel av typen sträng med namnet *SampleMessage* redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-176">In this sample, it is assumed that variables of type integer named *NumberOfIterations* and *NumberOfRunnings* and a variable of type string named *SampleMessage* have already been created.</span></span>

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a><span data-ttu-id="a5ebc-177">Ställa in och hämta ett komplext objekt i en variabel</span><span class="sxs-lookup"><span data-stu-id="a5ebc-177">Setting and retrieving a complex object in a variable</span></span>

<span data-ttu-id="a5ebc-178">hello följande exempel visas hur tooupdate en variabel med ett komplext värde i text-runbook.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-178">hello following sample code shows how tooupdate a variable with a complex value in a textual runbook.</span></span> <span data-ttu-id="a5ebc-179">I det här exemplet hämtas en virtuell Azure-dator med **Get-AzureVM** och sparade tooan befintlig Automation-variabel.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-179">In this sample, an Azure virtual machine is retrieved with **Get-AzureVM** and saved tooan existing Automation variable.</span></span>  <span data-ttu-id="a5ebc-180">Enligt beskrivningen i [datatyper](#variable-types), detta lagras som en PSCustomObject.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-180">As explained in [Variable types](#variable-types), this is stored as a PSCustomObject.</span></span>

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm


<span data-ttu-id="a5ebc-181">I följande kod hello, hämtas hello värdet från hello variabel och använda toostart hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-181">In hello following code, hello value is retrieved from hello variable and used toostart hello virtual machine.</span></span>

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a><span data-ttu-id="a5ebc-182">Ställa in och hämta en samling i en variabel</span><span class="sxs-lookup"><span data-stu-id="a5ebc-182">Setting and retrieving a collection in a variable</span></span>

<span data-ttu-id="a5ebc-183">hello följande exempel visas hur toouse en variabel med en samling med komplexa värden i text-runbook.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-183">hello following sample code shows how toouse a variable with a collection of complex values in a textual runbook.</span></span> <span data-ttu-id="a5ebc-184">I det här exemplet hämtas flera virtuella Azure-datorer med **Get-AzureVM** och sparade tooan befintlig Automation-variabel.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-184">In this sample, multiple Azure virtual machines are retrieved with **Get-AzureVM** and saved tooan existing Automation variable.</span></span>  <span data-ttu-id="a5ebc-185">Enligt beskrivningen i [datatyper](#variable-types), lagras detta som en samling PSCustomObjects.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-185">As explained in [Variable types](#variable-types), this is stored as a collection of PSCustomObjects.</span></span>

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

<span data-ttu-id="a5ebc-186">I följande kod hello, hello samlingen hämtas från hello variabeln och används toostart för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-186">In hello following code, hello collection is retrieved from hello variable and used toostart each virtual machine.</span></span>

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }


### <a name="graphical-runbook-samples"></a><span data-ttu-id="a5ebc-187">Grafiska runbook-exempel</span><span class="sxs-lookup"><span data-stu-id="a5ebc-187">Graphical runbook samples</span></span>

<span data-ttu-id="a5ebc-188">Lägg till hello i en grafisk runbook **Get-automationvariable,** eller **Set-automationvariable,** genom att högerklicka på hello variabeln i hello biblioteket rutan hello grafiska redigerare och välja hello aktivitet som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-188">In a graphical runbook, you add hello **Get-AutomationVariable** or **Set-AutomationVariable** by right-clicking on hello variable in hello Library pane of hello graphical editor and selecting hello activity you want.</span></span>

![Lägg till variabel toocanvas](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a><span data-ttu-id="a5ebc-190">Ange värden i en variabel</span><span class="sxs-lookup"><span data-stu-id="a5ebc-190">Setting values in a variable</span></span>
<span data-ttu-id="a5ebc-191">hello följande bild visar exempel aktiviteter tooupdate en variabel med ett enkelt värde i en grafisk runbook.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-191">hello following image shows sample activities tooupdate a variable with a simple value in a graphical runbook.</span></span> <span data-ttu-id="a5ebc-192">I det här exemplet hämtas en enda virtuell Azure-dator med **Get-AzureRmVM** och hello datornamn sparas tooan befintlig Automation-variabel av typen sträng.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-192">In this sample, a single Azure virtual machine is retrieved with **Get-AzureRmVM** and hello computer name is saved tooan existing Automation variable with a type of String.</span></span>  <span data-ttu-id="a5ebc-193">Det spelar ingen roll om hello [länken är en pipeline eller sekvens](automation-graphical-authoring-intro.md#links-and-workflow) eftersom vi räknar endast ett objekt i hello utdata.</span><span class="sxs-lookup"><span data-stu-id="a5ebc-193">It doesn't matter whether hello [link is a pipeline or sequence](automation-graphical-authoring-intro.md#links-and-workflow) since we only expect a single object in hello output.</span></span>

![Ange enkla variabel](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a><span data-ttu-id="a5ebc-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a5ebc-195">Next Steps</span></span>

* <span data-ttu-id="a5ebc-196">toolearn mer information om hur du ansluter aktiviteter tillsammans i grafiska redigering finns [länkar i grafiska redigering](automation-graphical-authoring-intro.md#links-and-workflow)</span><span class="sxs-lookup"><span data-stu-id="a5ebc-196">toolearn more about connecting activities together in graphical authoring, see [Links in graphical authoring](automation-graphical-authoring-intro.md#links-and-workflow)</span></span>
* <span data-ttu-id="a5ebc-197">tooget igång med grafiska runbooks finns [min första grafisk runbook](automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="a5ebc-197">tooget started with Graphical runbooks, see [My first graphical runbook](automation-first-runbook-graphical.md)</span></span> 

