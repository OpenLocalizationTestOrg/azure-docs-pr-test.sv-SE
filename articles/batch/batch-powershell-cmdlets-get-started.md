---
title: "Komma igång med PowerShell för Azure Batch | Microsoft Docs"
description: "En snabb introduktion till Azure PowerShell-cmdlets som du kan använda för att hantera Batch-resurserna."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: f9ad62c5-27bf-4e6b-a5bf-c5f5914e6199
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e33be6ed658e00250ea1e80cd7da4d348fb18296
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="manage-batch-resources-with-powershell-cmdlets"></a><span data-ttu-id="42e98-103">Hantera Batch-resurser med PowerShell-cmdletar</span><span class="sxs-lookup"><span data-stu-id="42e98-103">Manage Batch resources with PowerShell cmdlets</span></span>

<span data-ttu-id="42e98-104">Med Azure Batch PowerShell-cmdletarna kan du genomföra och skriva många av de uppgifter som du utför med Batch-API:erna, Azure Portal och Azures kommandoradsgränssnitt (CLI).</span><span class="sxs-lookup"><span data-stu-id="42e98-104">With the Azure Batch PowerShell cmdlets, you can perform and script many of the same tasks you carry out with the Batch APIs, the Azure portal, and the Azure Command-Line Interface (CLI).</span></span> <span data-ttu-id="42e98-105">Det här är en snabb introduktion till de cmdletar som du kan använda för att hantera Batch-konton och arbeta med Batch-resurser, t.ex. pooler, jobb och uppgifter.</span><span class="sxs-lookup"><span data-stu-id="42e98-105">This is a quick introduction to the cmdlets you can use to manage your Batch accounts and work with your Batch resources such as pools, jobs, and tasks.</span></span>

<span data-ttu-id="42e98-106">En fullständig lista över alla Batch-cmdlets och en detaljerad cmdlet-syntax finns i [Cmdlet-referens för Azure Batch](/powershell/module/azurerm.batch/#batch).</span><span class="sxs-lookup"><span data-stu-id="42e98-106">For a complete list of Batch cmdlets and detailed cmdlet syntax, see the [Azure Batch cmdlet reference](/powershell/module/azurerm.batch/#batch).</span></span>

<span data-ttu-id="42e98-107">Den här artikeln baseras på cmdlet:ar i Azure PowerShell, version 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="42e98-107">This article is based on cmdlets in Azure PowerShell version 3.0.0.</span></span> <span data-ttu-id="42e98-108">Vi rekommenderar att du uppdaterar Azure PowerShell ofta för att dra nytta av tjänstuppdateringar och förbättringar.</span><span class="sxs-lookup"><span data-stu-id="42e98-108">We recommend that you update your Azure PowerShell frequently to take advantage of service updates and enhancements.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42e98-109">Krav</span><span class="sxs-lookup"><span data-stu-id="42e98-109">Prerequisites</span></span>
<span data-ttu-id="42e98-110">Utför följande åtgärder för att använda Azure PowerShell för att hantera dina Batch-resurser.</span><span class="sxs-lookup"><span data-stu-id="42e98-110">Perform the following operations to use Azure PowerShell to manage your Batch resources.</span></span>

* <span data-ttu-id="42e98-111">Installera och konfigurera [Azure PowerShell](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="42e98-111">[Install and configure Azure PowerShell](/powershell/azure/overview)</span></span>
* <span data-ttu-id="42e98-112">Kör cmdlet:en **Login-AzureRmAccount** för att ansluta till din prenumeration (Azure Batch-cmdlet:ar levereras i modulen Azure Resource Manager):</span><span class="sxs-lookup"><span data-stu-id="42e98-112">Run the **Login-AzureRmAccount** cmdlet to connect to your subscription (the Azure Batch cmdlets ship in the Azure Resource Manager module):</span></span>
  
    `Login-AzureRmAccount`
* <span data-ttu-id="42e98-113">**Registrera med Batch-leverantörens namnområde**.</span><span class="sxs-lookup"><span data-stu-id="42e98-113">**Register with the Batch provider namespace**.</span></span> <span data-ttu-id="42e98-114">Den här åtgärden behöver bara utföras **en gång per prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="42e98-114">This operation only needs to be performed **once per subscription**.</span></span>
  
    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a><span data-ttu-id="42e98-115">Hantera Batch-konton och nycklar</span><span class="sxs-lookup"><span data-stu-id="42e98-115">Manage Batch accounts and keys</span></span>
### <a name="create-a-batch-account"></a><span data-ttu-id="42e98-116">Skapa ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="42e98-116">Create a Batch account</span></span>
<span data-ttu-id="42e98-117">**New-AzureRmBatchAccount** skapar ett Batch-konto i en angiven resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="42e98-117">**New-AzureRmBatchAccount** creates a Batch account in a specified resource group.</span></span> <span data-ttu-id="42e98-118">Om du inte redan har en resursgrupp, skapa en genom att köra cmdlet:en [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="42e98-118">If you don't already have a resource group, create one by running the [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span></span> <span data-ttu-id="42e98-119">Ange ett av Azure-områdena i parametern **Plats**, till exempel "USA, centrala".</span><span class="sxs-lookup"><span data-stu-id="42e98-119">Specify one of the Azure regions in the **Location** parameter, such as "Central US".</span></span> <span data-ttu-id="42e98-120">Exempel:</span><span class="sxs-lookup"><span data-stu-id="42e98-120">For example:</span></span>

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

<span data-ttu-id="42e98-121">Skapa sedan ett Batch-konto i resursgruppen och ange ett namn för kontot i <*account_name*> och din resursgrupps namn och plats.</span><span class="sxs-lookup"><span data-stu-id="42e98-121">Then, create a Batch account in the resource group, specifying a name for the account in <*account_name*> and the location and name of your resource group.</span></span> <span data-ttu-id="42e98-122">Det kan ta en stund innan skapandet av Batch-kontot har slutförts.</span><span class="sxs-lookup"><span data-stu-id="42e98-122">Creating the Batch account can take some time to complete.</span></span> <span data-ttu-id="42e98-123">Exempel:</span><span class="sxs-lookup"><span data-stu-id="42e98-123">For example:</span></span>

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [!NOTE]
> <span data-ttu-id="42e98-124">Batch-kontonamnet måste vara unikt för Azure-regionen för resursgruppen, innehålla mellan 3 och 24 tecken och endast bestå av små bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="42e98-124">The Batch account name must be unique to the Azure region for the resource group, contain between 3 and 24 characters, and use lowercase letters and numbers only.</span></span>
> 
> 

### <a name="get-account-access-keys"></a><span data-ttu-id="42e98-125">Hämta kontoåtkomstnycklar</span><span class="sxs-lookup"><span data-stu-id="42e98-125">Get account access keys</span></span>
<span data-ttu-id="42e98-126">**Get-AzureRmBatchAccountKeys** visar åtkomstnycklarna som är associerade med ett Azure Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="42e98-126">**Get-AzureRmBatchAccountKeys** shows the access keys associated with an Azure Batch account.</span></span> <span data-ttu-id="42e98-127">Kör till exempel följande för att hämta de primära och sekundära nycklarna för det konto som du skapade.</span><span class="sxs-lookup"><span data-stu-id="42e98-127">For example, run the following to get the primary and secondary keys of the account you created.</span></span>

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a><span data-ttu-id="42e98-128">Generera en ny åtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="42e98-128">Generate a new access key</span></span>
<span data-ttu-id="42e98-129">**New-AzureRmBatchAccountKey** genererar en ny primär eller sekundär kontonyckel för ett Azure Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="42e98-129">**New-AzureRmBatchAccountKey** generates a new primary or secondary account key for an Azure Batch account.</span></span> <span data-ttu-id="42e98-130">Om du till exempel vill skapa en ny primär nyckel för Batch-kontot skriver du:</span><span class="sxs-lookup"><span data-stu-id="42e98-130">For example, to generate a new primary key for your Batch account, type:</span></span>

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [!NOTE]
> <span data-ttu-id="42e98-131">Om du vill skapa en ny sekundär nyckel anger du ”Secondary” för parametern **KeyType**.</span><span class="sxs-lookup"><span data-stu-id="42e98-131">To generate a new secondary key, specify "Secondary" for the **KeyType** parameter.</span></span> <span data-ttu-id="42e98-132">Du måste återskapa de primära och sekundära nycklarna separat.</span><span class="sxs-lookup"><span data-stu-id="42e98-132">You have to regenerate the primary and secondary keys separately.</span></span>
> 
> 

### <a name="delete-a-batch-account"></a><span data-ttu-id="42e98-133">Ta bort ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="42e98-133">Delete a Batch account</span></span>
<span data-ttu-id="42e98-134">**Remove-AzureRmBatchAccount** tar bort ett Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="42e98-134">**Remove-AzureRmBatchAccount** deletes a Batch account.</span></span> <span data-ttu-id="42e98-135">Till exempel:</span><span class="sxs-lookup"><span data-stu-id="42e98-135">For example:</span></span>

    Remove-AzureRmBatchAccount -AccountName <account_name>

<span data-ttu-id="42e98-136">När du uppmanas att göra det bekräftar du att du vill ta bort kontot.</span><span class="sxs-lookup"><span data-stu-id="42e98-136">When prompted, confirm you want to remove the account.</span></span> <span data-ttu-id="42e98-137">Det kan ta en stund innan kontot tagits bort.</span><span class="sxs-lookup"><span data-stu-id="42e98-137">Account removal can take some time to complete.</span></span>

## <a name="create-a-batchaccountcontext-object"></a><span data-ttu-id="42e98-138">Skapa ett BatchAccountContext-objekt</span><span class="sxs-lookup"><span data-stu-id="42e98-138">Create a BatchAccountContext object</span></span>
<span data-ttu-id="42e98-139">Om du vill autentisera med Batch PowerShell-cmdlets när du skapar och hanterar pooler, jobb, aktiviteter och andra resurser i Batch skapar du ett BatchAccountContext-objekt för att lagra ditt kontonamn och dina nycklar:</span><span class="sxs-lookup"><span data-stu-id="42e98-139">To authenticate using the Batch PowerShell cmdlets when you create and manage Batch pools, jobs, tasks, and other resources, first create a BatchAccountContext object to store your account name and keys:</span></span>

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

<span data-ttu-id="42e98-140">Du skickar BatchAccountContext-objektet till cmdlets som använder parametern **BatchContext**.</span><span class="sxs-lookup"><span data-stu-id="42e98-140">You pass the BatchAccountContext object into cmdlets that use the **BatchContext** parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="42e98-141">Som standard används kontots primära nyckel för autentisering, men du kan uttryckligen välja vilken nyckel som ska användas genom att ändra BatchAccountContext-objektets **KeyInUse**-egenskap: `$context.KeyInUse = "Secondary"`.</span><span class="sxs-lookup"><span data-stu-id="42e98-141">By default, the account's primary key is used for authentication, but you can explicitly select the key to use by changing your BatchAccountContext object’s **KeyInUse** property: `$context.KeyInUse = "Secondary"`.</span></span>
> 
> 

## <a name="create-and-modify-batch-resources"></a><span data-ttu-id="42e98-142">Skapa och ändra Batch-resurser</span><span class="sxs-lookup"><span data-stu-id="42e98-142">Create and modify Batch resources</span></span>
<span data-ttu-id="42e98-143">Skapa resurser under ett Batch-konto genom att använda cmdletar som **New-AzureBatchPool**, **New-AzureBatchJob** och **New-AzureBatchTask**.</span><span class="sxs-lookup"><span data-stu-id="42e98-143">Use cmdlets such as **New-AzureBatchPool**, **New-AzureBatchJob**, and **New-AzureBatchTask** to create resources under a Batch account.</span></span> <span data-ttu-id="42e98-144">Det finns motsvarande cmdlets för **Get-** och **Set-** som du kan använda för att uppdatera egenskaperna för befintliga resurser, och cmdlets för **Remove-** som du använder om du vill ta bort resurser under ett Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="42e98-144">There are corresponding **Get-** and **Set-** cmdlets to update the properties of existing resources, and  **Remove-** cmdlets to remove resources under a Batch account.</span></span>

<span data-ttu-id="42e98-145">När du använder många av dessa cmdletar måste du, förutom att skicka ett BatchContext-objekt, skapa eller skicka objekt som innehåller detaljerade resursinställningar, så som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="42e98-145">When using many of these cmdlets, in addition to passing a BatchContext object, you need to create or pass objects that contain detailed resource settings, as shown in the following example.</span></span> <span data-ttu-id="42e98-146">Visa den detaljerade hjälpinformationen för respektive cmdlet om du vill ha fler exempel.</span><span class="sxs-lookup"><span data-stu-id="42e98-146">See the detailed help for each cmdlet for additional examples.</span></span>

### <a name="create-a-batch-pool"></a><span data-ttu-id="42e98-147">Skapa en Batch-pool</span><span class="sxs-lookup"><span data-stu-id="42e98-147">Create a Batch pool</span></span>
<span data-ttu-id="42e98-148">När du skapar eller uppdaterar en Batch-pool väljer du antingen en molntjänstkonfiguration eller en konfiguration för virtuell dator för beräkningsnodernas operativsystem (se [Översikt över Batch-funktioner](batch-api-basics.md#pool)).</span><span class="sxs-lookup"><span data-stu-id="42e98-148">When creating or updating a Batch pool, you select either the cloud service configuration or the virtual machine configuration for the operating system on the compute nodes (see [Batch feature overview](batch-api-basics.md#pool)).</span></span> <span data-ttu-id="42e98-149">Om du anger konfigurationen för molntjänsten avbildas dina beräkningsnoder med någon av [versionerna av Azures gäst-OS](../cloud-services/cloud-services-guestos-update-matrix.md#releases).</span><span class="sxs-lookup"><span data-stu-id="42e98-149">If you specify the cloud service configuration, your compute nodes are imaged with one of the [Azure Guest OS releases](../cloud-services/cloud-services-guestos-update-matrix.md#releases).</span></span> <span data-ttu-id="42e98-150">Om du anger konfigurationen för den virtuella datorn kan du antingen ange någon av VM-avbildningarna som stöds av Linux eller Windows som anges på [Azure Virtual Machines Marketplace][vm_marketplace] eller ange en anpassad avbildning som du har förberett.</span><span class="sxs-lookup"><span data-stu-id="42e98-150">If you specify the virtual machine configuration, you can either specify one of the supported Linux or Windows VM images listed in the [Azure Virtual Machines Marketplace][vm_marketplace], or provide a custom image that you have prepared.</span></span>

<span data-ttu-id="42e98-151">När du kör **New-AzureBatchPool**, så överför operativsystemsinställningarna i en PSCloudServiceConfiguration eller ett PSVirtualMachineConfiguration-objekt.</span><span class="sxs-lookup"><span data-stu-id="42e98-151">When you run **New-AzureBatchPool**, pass the operating system settings in a PSCloudServiceConfiguration or PSVirtualMachineConfiguration object.</span></span> <span data-ttu-id="42e98-152">Följande cmdlet skapar t.ex. en ny Batch-pool med små beräkningsnoder i molntjänstkonfigurationen, avbildade med den senaste operativsystemsversionen för familj 3 (Windows Server 2012).</span><span class="sxs-lookup"><span data-stu-id="42e98-152">For example, the following cmdlet creates a new Batch pool with size Small compute nodes in the cloud service configuration, imaged with the latest operating system version of family 3 (Windows Server 2012).</span></span> <span data-ttu-id="42e98-153">Här anger parametern **CloudServiceConfiguration** variabeln *$configuration* som PSCloudServiceConfiguration-objektet.</span><span class="sxs-lookup"><span data-stu-id="42e98-153">Here, the **CloudServiceConfiguration** parameter specifies the *$configuration* variable as the PSCloudServiceConfiguration object.</span></span> <span data-ttu-id="42e98-154">Parametern **BatchContext** anger en tidigare definierad variabel, *$context*, som BatchAccountContext-objektet.</span><span class="sxs-lookup"><span data-stu-id="42e98-154">The **BatchContext** parameter specifies a previously defined variable *$context* as the BatchAccountContext object.</span></span>

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

<span data-ttu-id="42e98-155">Beräkningsnodernas målantal i den nya poolen bestäms av en autoskalningsformel.</span><span class="sxs-lookup"><span data-stu-id="42e98-155">The target number of compute nodes in the new pool is determined by an autoscaling formula.</span></span> <span data-ttu-id="42e98-156">I det här fallet är formeln helt enkelt **$TargetDedicated=4**, vilket indikerar att antalet beräkningsnoder i poolen som mest är 4.</span><span class="sxs-lookup"><span data-stu-id="42e98-156">In this case, the formula is simply **$TargetDedicated=4**, indicating the number of compute nodes in the pool is 4 at most.</span></span>

## <a name="query-for-pools-jobs-tasks-and-other-details"></a><span data-ttu-id="42e98-157">Fråga avseende pooler, jobb, uppgifter och annan information</span><span class="sxs-lookup"><span data-stu-id="42e98-157">Query for pools, jobs, tasks, and other details</span></span>
<span data-ttu-id="42e98-158">Använd cmdlets som **Get-AzureBatchPool**, **Get-AzureBatchJob** och **Get-AzureBatchTask** om du vill fråga efter entiteter som skapats under ett Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="42e98-158">Use cmdlets such as **Get-AzureBatchPool**, **Get-AzureBatchJob**, and **Get-AzureBatchTask** to query for entities created under a Batch account.</span></span>

### <a name="query-for-data"></a><span data-ttu-id="42e98-159">Fråga efter data</span><span class="sxs-lookup"><span data-stu-id="42e98-159">Query for data</span></span>
<span data-ttu-id="42e98-160">Använd till exempel **Get-AzureBatchPools** om du vill hämta dina pooler.</span><span class="sxs-lookup"><span data-stu-id="42e98-160">As an example, use **Get-AzureBatchPools** to find your pools.</span></span> <span data-ttu-id="42e98-161">Som standard returneras alla pooler i ditt konto, förutsatt att du redan har lagrat BatchAccountContext-objektet i *$context*:</span><span class="sxs-lookup"><span data-stu-id="42e98-161">By default this queries for all pools under your account, assuming you already stored the BatchAccountContext object in *$context*:</span></span>

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a><span data-ttu-id="42e98-162">Använda ett OData-filter</span><span class="sxs-lookup"><span data-stu-id="42e98-162">Use an OData filter</span></span>
<span data-ttu-id="42e98-163">Du kan lägga till ett OData-filter med parametern **Filter** om du bara vill söka efter de objekt som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="42e98-163">You can supply an OData filter using the **Filter** parameter to find only the objects you’re interested in.</span></span> <span data-ttu-id="42e98-164">Du kan till exempel hämta alla pooler med ID:n som börjar med ”myPool”:</span><span class="sxs-lookup"><span data-stu-id="42e98-164">For example, you can find all pools with ids starting with “myPool”:</span></span>

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

<span data-ttu-id="42e98-165">Den här metoden är inte lika flexibel som ”Where-Object”-metoden i en lokal pipeline.</span><span class="sxs-lookup"><span data-stu-id="42e98-165">This method is not as flexible as using “Where-Object” in a local pipeline.</span></span> <span data-ttu-id="42e98-166">Frågan skickas dock direkt till Batch-tjänsten så att all filtrering utförs på serversidan, vilket sparar Internetbandbredd.</span><span class="sxs-lookup"><span data-stu-id="42e98-166">However, the query gets sent to the Batch service directly so that all filtering happens on the server side, saving Internet bandwidth.</span></span>

### <a name="use-the-id-parameter"></a><span data-ttu-id="42e98-167">Använda Id-parametern</span><span class="sxs-lookup"><span data-stu-id="42e98-167">Use the Id parameter</span></span>
<span data-ttu-id="42e98-168">Ett alternativ till ett OData-filter är att använda parametern **Id**.</span><span class="sxs-lookup"><span data-stu-id="42e98-168">An alternative to an OData filter is to use the **Id** parameter.</span></span> <span data-ttu-id="42e98-169">Så här frågar du efter en specifik pool med ID:t ”myPool”:</span><span class="sxs-lookup"><span data-stu-id="42e98-169">To query for a specific pool with id "myPool":</span></span>

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

<span data-ttu-id="42e98-170">Parametern **Id** stöder endast sökningar efter fullständiga ID:n, inte jokertecken eller OData-filter.</span><span class="sxs-lookup"><span data-stu-id="42e98-170">The **Id** parameter supports only full-id search, not wildcards or OData-style filters.</span></span>

### <a name="use-the-maxcount-parameter"></a><span data-ttu-id="42e98-171">Använda parametern MaxCount</span><span class="sxs-lookup"><span data-stu-id="42e98-171">Use the MaxCount parameter</span></span>
<span data-ttu-id="42e98-172">Som standard returnerar varje cmdlet högst 1 000 objekt.</span><span class="sxs-lookup"><span data-stu-id="42e98-172">By default, each cmdlet returns a maximum of 1000 objects.</span></span> <span data-ttu-id="42e98-173">Om du når den här gränsen kan du antingen förfina filtret så att färre objekt returneras eller uttryckligen ställa in ett högsta antal med parametern **MaxCount**.</span><span class="sxs-lookup"><span data-stu-id="42e98-173">If you reach this limit, either refine your filter to bring back fewer objects, or explicitly set a maximum using the **MaxCount** parameter.</span></span> <span data-ttu-id="42e98-174">Till exempel:</span><span class="sxs-lookup"><span data-stu-id="42e98-174">For example:</span></span>

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

<span data-ttu-id="42e98-175">Om du vill ta bort den övre gränsen anger du **MaxCount** till 0 eller mindre.</span><span class="sxs-lookup"><span data-stu-id="42e98-175">To remove the upper bound, set **MaxCount** to 0 or less.</span></span>

### <a name="use-the-powershell-pipeline"></a><span data-ttu-id="42e98-176">Använd PowerShell pipeline</span><span class="sxs-lookup"><span data-stu-id="42e98-176">Use the PowerShell pipeline</span></span>
<span data-ttu-id="42e98-177">Batch-cmdlets kan utnyttja PowerShell-pipelinen för att skicka data mellan cmdlets.</span><span class="sxs-lookup"><span data-stu-id="42e98-177">Batch cmdlets can leverage the PowerShell pipeline to send data between cmdlets.</span></span> <span data-ttu-id="42e98-178">Detta har samma effekt som om du anger en parameter men gör det enklare att arbeta med flera entiteter.</span><span class="sxs-lookup"><span data-stu-id="42e98-178">This has the same effect as specifying a parameter, but makes working with multiple entities easier.</span></span>

<span data-ttu-id="42e98-179">Exempelvis söka efter och visa alla aktiviteter på ditt konto:</span><span class="sxs-lookup"><span data-stu-id="42e98-179">For example, find and display all tasks under your account:</span></span>

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

<span data-ttu-id="42e98-180">Starta om varje beräkningsnod i en pool:</span><span class="sxs-lookup"><span data-stu-id="42e98-180">Restart (reboot) every compute node in a pool:</span></span>

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a><span data-ttu-id="42e98-181">Hantera programpaket</span><span class="sxs-lookup"><span data-stu-id="42e98-181">Application package management</span></span>
<span data-ttu-id="42e98-182">Programpaket är ett förenklat sätt att distribuera program till beräkningsnoder i dina pooler.</span><span class="sxs-lookup"><span data-stu-id="42e98-182">Application packages provide a simplified way to deploy applications to the compute nodes in your pools.</span></span> <span data-ttu-id="42e98-183">Med Batch-PowerShell-cmdlet:ar kan du överföra och hantera programpaket i Batch-kontot och distribuera paketversioner för att beräkna noder.</span><span class="sxs-lookup"><span data-stu-id="42e98-183">With the Batch PowerShell cmdlets, you can upload and manage application packages in your Batch account, and deploy package versions to compute nodes.</span></span>

<span data-ttu-id="42e98-184">**Skapa** ett program:</span><span class="sxs-lookup"><span data-stu-id="42e98-184">**Create** an application:</span></span>

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

<span data-ttu-id="42e98-185">**Lägg till** ett programpaket:</span><span class="sxs-lookup"><span data-stu-id="42e98-185">**Add** an application package:</span></span>

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

<span data-ttu-id="42e98-186">Ange **standardversionen** för programmet:</span><span class="sxs-lookup"><span data-stu-id="42e98-186">Set the **default version** for the application:</span></span>

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

<span data-ttu-id="42e98-187">**Lista** ett programs paket</span><span class="sxs-lookup"><span data-stu-id="42e98-187">**List** an application's packages</span></span>

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

<span data-ttu-id="42e98-188">**Ta bort** ett programpaket</span><span class="sxs-lookup"><span data-stu-id="42e98-188">**Delete** an application package</span></span>

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

<span data-ttu-id="42e98-189">**Ta bort** ett program</span><span class="sxs-lookup"><span data-stu-id="42e98-189">**Delete** an application</span></span>

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

> [!NOTE]
> <span data-ttu-id="42e98-190">Du måste ta bort alla programpaketversioner innan du tar bort ett program.</span><span class="sxs-lookup"><span data-stu-id="42e98-190">You must delete all of an application's application package versions before you delete the application.</span></span> <span data-ttu-id="42e98-191">Du får ett konfliktfel om du försöker ta bort ett program som för närvarande har programpaket.</span><span class="sxs-lookup"><span data-stu-id="42e98-191">You will receive a 'Conflict' error if you try to delete an application that currently has application packages.</span></span>
> 
> 

### <a name="deploy-an-application-package"></a><span data-ttu-id="42e98-192">Distribuera ett programpaket</span><span class="sxs-lookup"><span data-stu-id="42e98-192">Deploy an application package</span></span>
<span data-ttu-id="42e98-193">Du kan ange ett eller flera programpaket för distribution när du skapar en pool.</span><span class="sxs-lookup"><span data-stu-id="42e98-193">You can specify one or more application packages for deployment when you create a pool.</span></span> <span data-ttu-id="42e98-194">När du anger ett paket när poolen skapas distribueras den till varje nod när noden ansluter till poolen.</span><span class="sxs-lookup"><span data-stu-id="42e98-194">When you specify a package at pool creation time, it is deployed to each node as the node joins pool.</span></span> <span data-ttu-id="42e98-195">Paket distribueras också när en nod startas om eller när en avbildning återställs.</span><span class="sxs-lookup"><span data-stu-id="42e98-195">Packages are also deployed when a node is rebooted or reimaged.</span></span>

<span data-ttu-id="42e98-196">Ange alternativet `-ApplicationPackageReference` när du skapar en pool för att distribuera ett programpaket till poolen noder efterhand som de ansluts till poolen.</span><span class="sxs-lookup"><span data-stu-id="42e98-196">Specify the `-ApplicationPackageReference` option when creating a pool to deploy an application package to the pool's nodes as they join the pool.</span></span> <span data-ttu-id="42e98-197">Skapa först ett **PSApplicationPackageReference**-objekt och konfigurera det med program-Id och den paketversion som du vill distribuera till i poolens beräkningsnoder:</span><span class="sxs-lookup"><span data-stu-id="42e98-197">First, create a **PSApplicationPackageReference** object, and configure it with the application Id and package version you want to deploy to the pool's compute nodes:</span></span>

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

<span data-ttu-id="42e98-198">Skapa nu poolen och ange paketreferensobjekt som argument till `ApplicationPackageReferences`-alternativet:</span><span class="sxs-lookup"><span data-stu-id="42e98-198">Now create the pool, and specify the package reference object as the argument to the `ApplicationPackageReferences` option:</span></span>

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

<span data-ttu-id="42e98-199">Mer information om programpaket finns i [Deploy applications to compute nodes with Batch application packages](batch-application-packages.md) (Distribuera program till beräkningsnoder med Batch-programpaket).</span><span class="sxs-lookup"><span data-stu-id="42e98-199">You can find more information on application packages in [Deploy applications to compute nodes with Batch application packages](batch-application-packages.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42e98-200">Du måste [koppla ett Azure Storage-konto](#linked-storage-account-autostorage) till Batch-kontot för att använda programpaket.</span><span class="sxs-lookup"><span data-stu-id="42e98-200">You must [link an Azure Storage account](#linked-storage-account-autostorage) to your Batch account to use application packages.</span></span>
> 
> 

### <a name="update-a-pools-application-packages"></a><span data-ttu-id="42e98-201">Uppdatera programpaket för en pool</span><span class="sxs-lookup"><span data-stu-id="42e98-201">Update a pool's application packages</span></span>
<span data-ttu-id="42e98-202">Om du vill uppdatera program som är tilldelade till en befintlig pool skapar du ett PSApplicationPackageReference-objekt med egenskaperna (program-Id och paketversion):</span><span class="sxs-lookup"><span data-stu-id="42e98-202">To update the applications assigned to an existing pool, first create a PSApplicationPackageReference object with the desired properties (application Id and package version):</span></span>

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

<span data-ttu-id="42e98-203">Sedan hämtar du poolen från Batch, tar bort eventuella befintliga paket, lägger till vår nya paketreferens och uppdaterat Batch-tjänsten med de nya poolinställningarna:</span><span class="sxs-lookup"><span data-stu-id="42e98-203">Next, get the pool from Batch, clear out any existing packages, add our new package reference, and update the Batch service with the new pool settings:</span></span>

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

<span data-ttu-id="42e98-204">Poolens egenskaper i Batch-tjänsten har nu uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="42e98-204">You've now updated the pool's properties in the Batch service.</span></span> <span data-ttu-id="42e98-205">Du måste starta om eller återställa avbildningen av noder om du vill distribuera nya programpaket för beräkningsnoder i poolen.</span><span class="sxs-lookup"><span data-stu-id="42e98-205">To actually deploy the new application package to compute nodes in the pool, however, you must restart or reimage those nodes.</span></span> <span data-ttu-id="42e98-206">Du kan starta om varje nod i en pool med det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="42e98-206">You can restart every node in a pool with this command:</span></span>

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

> [!TIP]
> <span data-ttu-id="42e98-207">Du kan distribuera flera programpaket till beräkningsnoder i poolen.</span><span class="sxs-lookup"><span data-stu-id="42e98-207">You can deploy multiple application packages to the compute nodes in a pool.</span></span> <span data-ttu-id="42e98-208">Om du vill *lägga till* ett programpaket i stället för att ersätta de för närvarande distribuerade paketen, utelämnar du raden `$pool.ApplicationPackageReferences.Clear()` ovan.</span><span class="sxs-lookup"><span data-stu-id="42e98-208">If you'd like to *add* an application package instead of replacing the currently deployed packages, omit the `$pool.ApplicationPackageReferences.Clear()` line above.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="42e98-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="42e98-209">Next steps</span></span>
* <span data-ttu-id="42e98-210">Detaljerad cmdlet-syntax och exempel finns i [Cmdlet-referens för Azure Batch](/powershell/module/azurerm.batch/#batch).</span><span class="sxs-lookup"><span data-stu-id="42e98-210">For detailed cmdlet syntax and examples, see [Azure Batch cmdlet reference](/powershell/module/azurerm.batch/#batch).</span></span>
* <span data-ttu-id="42e98-211">Mer information om program och programpaket i Batch finns i [Deploy applications to compute nodes with Batch application packages](batch-application-packages.md) (Distribuera program till beräkningsnoder med Batch-programpaket).</span><span class="sxs-lookup"><span data-stu-id="42e98-211">For more information about applications and application packages in Batch, see [Deploy applications to compute nodes with Batch application packages](batch-application-packages.md).</span></span>

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/