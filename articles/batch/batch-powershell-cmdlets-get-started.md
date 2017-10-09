---
title: "aaaGet igång med PowerShell för Azure Batch | Microsoft Docs"
description: "En snabb introduktion toohello Azure PowerShell-cmdlets som du kan använda toomanage Batch resurser."
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
ms.openlocfilehash: 3e4d12e9c1e52a5b2db2dd44346edda93b7ef92b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-powershell-cmdlets"></a><span data-ttu-id="122e6-103">Hantera Batch-resurser med PowerShell-cmdletar</span><span class="sxs-lookup"><span data-stu-id="122e6-103">Manage Batch resources with PowerShell cmdlets</span></span>

<span data-ttu-id="122e6-104">Med hello Azure Batch-PowerShell-cmdlets, du kan utföra och skript många hello samma uppgifter som du vill utföra med hello Batch-API: er, hello Azure-portalen och hello Azure-kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="122e6-104">With hello Azure Batch PowerShell cmdlets, you can perform and script many of hello same tasks you carry out with hello Batch APIs, hello Azure portal, and hello Azure Command-Line Interface (CLI).</span></span> <span data-ttu-id="122e6-105">Det här är en snabb introduktion toohello cmdlets som du kan använda toomanage Batch-konton och arbeta med resurserna Batch som pooler, jobb och uppgifter.</span><span class="sxs-lookup"><span data-stu-id="122e6-105">This is a quick introduction toohello cmdlets you can use toomanage your Batch accounts and work with your Batch resources such as pools, jobs, and tasks.</span></span>

<span data-ttu-id="122e6-106">En fullständig lista över Batch-cmdlets och detaljerade cmdlet syntax finns hello [cmdlet-referens för Azure Batch](/powershell/module/azurerm.batch/#batch).</span><span class="sxs-lookup"><span data-stu-id="122e6-106">For a complete list of Batch cmdlets and detailed cmdlet syntax, see hello [Azure Batch cmdlet reference](/powershell/module/azurerm.batch/#batch).</span></span>

<span data-ttu-id="122e6-107">Den här artikeln baseras på cmdlet:ar i Azure PowerShell, version 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="122e6-107">This article is based on cmdlets in Azure PowerShell version 3.0.0.</span></span> <span data-ttu-id="122e6-108">Vi rekommenderar att du uppdaterar din Azure PowerShell ofta tootake nytta av tjänsteuppdateringar och förbättringar.</span><span class="sxs-lookup"><span data-stu-id="122e6-108">We recommend that you update your Azure PowerShell frequently tootake advantage of service updates and enhancements.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="122e6-109">Krav</span><span class="sxs-lookup"><span data-stu-id="122e6-109">Prerequisites</span></span>
<span data-ttu-id="122e6-110">Utför följande åtgärder toouse Azure PowerShell toomanage hello Batch-resurser.</span><span class="sxs-lookup"><span data-stu-id="122e6-110">Perform hello following operations toouse Azure PowerShell toomanage your Batch resources.</span></span>

* <span data-ttu-id="122e6-111">Installera och konfigurera [Azure PowerShell](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="122e6-111">[Install and configure Azure PowerShell](/powershell/azure/overview)</span></span>
* <span data-ttu-id="122e6-112">Kör hello **Login-AzureRmAccount** cmdlet tooconnect tooyour prenumeration (hello Azure Batch-cmdlets leverera i hello Azure Resource Manager-modulen):</span><span class="sxs-lookup"><span data-stu-id="122e6-112">Run hello **Login-AzureRmAccount** cmdlet tooconnect tooyour subscription (hello Azure Batch cmdlets ship in hello Azure Resource Manager module):</span></span>
  
    `Login-AzureRmAccount`
* <span data-ttu-id="122e6-113">**Registrera med hello Batch leverantörens namnrymd**.</span><span class="sxs-lookup"><span data-stu-id="122e6-113">**Register with hello Batch provider namespace**.</span></span> <span data-ttu-id="122e6-114">Den här åtgärden behöver bara utföras toobe **en gång per prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="122e6-114">This operation only needs toobe performed **once per subscription**.</span></span>
  
    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a><span data-ttu-id="122e6-115">Hantera Batch-konton och nycklar</span><span class="sxs-lookup"><span data-stu-id="122e6-115">Manage Batch accounts and keys</span></span>
### <a name="create-a-batch-account"></a><span data-ttu-id="122e6-116">Skapa ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="122e6-116">Create a Batch account</span></span>
<span data-ttu-id="122e6-117">**New-AzureRmBatchAccount** skapar ett Batch-konto i en angiven resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="122e6-117">**New-AzureRmBatchAccount** creates a Batch account in a specified resource group.</span></span> <span data-ttu-id="122e6-118">Om du inte redan har en resursgrupp, skapa en genom att köra hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="122e6-118">If you don't already have a resource group, create one by running hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span></span> <span data-ttu-id="122e6-119">Ange en hello Azure regioner i hello **plats** parameter, till exempel ”centrala USA”.</span><span class="sxs-lookup"><span data-stu-id="122e6-119">Specify one of hello Azure regions in hello **Location** parameter, such as "Central US".</span></span> <span data-ttu-id="122e6-120">Exempel:</span><span class="sxs-lookup"><span data-stu-id="122e6-120">For example:</span></span>

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

<span data-ttu-id="122e6-121">Skapa sedan en Batch-kontot i hello resursgruppen att ange ett namn för hello-konto <*kontonamn*> och hello plats och namn för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="122e6-121">Then, create a Batch account in hello resource group, specifying a name for hello account in <*account_name*> and hello location and name of your resource group.</span></span> <span data-ttu-id="122e6-122">Skapa hello Batch-kontot kan ta viss tid toocomplete.</span><span class="sxs-lookup"><span data-stu-id="122e6-122">Creating hello Batch account can take some time toocomplete.</span></span> <span data-ttu-id="122e6-123">Exempel:</span><span class="sxs-lookup"><span data-stu-id="122e6-123">For example:</span></span>

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [!NOTE]
> <span data-ttu-id="122e6-124">hello Batch-kontot måste vara unika toohello Azure-region för hello resursgruppen innehålla mellan 3 och 24 tecken och använda gemena bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="122e6-124">hello Batch account name must be unique toohello Azure region for hello resource group, contain between 3 and 24 characters, and use lowercase letters and numbers only.</span></span>
> 
> 

### <a name="get-account-access-keys"></a><span data-ttu-id="122e6-125">Hämta kontoåtkomstnycklar</span><span class="sxs-lookup"><span data-stu-id="122e6-125">Get account access keys</span></span>
<span data-ttu-id="122e6-126">**Get-AzureRmBatchAccountKeys** visar hello snabbtangenter som är associerad med ett Azure Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="122e6-126">**Get-AzureRmBatchAccountKeys** shows hello access keys associated with an Azure Batch account.</span></span> <span data-ttu-id="122e6-127">Till exempel hello följande tooget hello primära och sekundära nycklarna för hello-konto som du skapade.</span><span class="sxs-lookup"><span data-stu-id="122e6-127">For example, run hello following tooget hello primary and secondary keys of hello account you created.</span></span>

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a><span data-ttu-id="122e6-128">Generera en ny åtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="122e6-128">Generate a new access key</span></span>
<span data-ttu-id="122e6-129">**New-AzureRmBatchAccountKey** genererar en ny primär eller sekundär kontonyckel för ett Azure Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="122e6-129">**New-AzureRmBatchAccountKey** generates a new primary or secondary account key for an Azure Batch account.</span></span> <span data-ttu-id="122e6-130">Till exempel toogenerate en ny primär nyckel för Batch-kontot, skriver du:</span><span class="sxs-lookup"><span data-stu-id="122e6-130">For example, toogenerate a new primary key for your Batch account, type:</span></span>

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [!NOTE]
> <span data-ttu-id="122e6-131">”Sekundär” anger du toogenerate en ny sekundär nyckel för hello **KeyType** parameter.</span><span class="sxs-lookup"><span data-stu-id="122e6-131">toogenerate a new secondary key, specify "Secondary" for hello **KeyType** parameter.</span></span> <span data-ttu-id="122e6-132">Du måste separat tooregenerate hello primära och sekundära nycklarna.</span><span class="sxs-lookup"><span data-stu-id="122e6-132">You have tooregenerate hello primary and secondary keys separately.</span></span>
> 
> 

### <a name="delete-a-batch-account"></a><span data-ttu-id="122e6-133">Ta bort ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="122e6-133">Delete a Batch account</span></span>
<span data-ttu-id="122e6-134">**Remove-AzureRmBatchAccount** tar bort ett Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="122e6-134">**Remove-AzureRmBatchAccount** deletes a Batch account.</span></span> <span data-ttu-id="122e6-135">Exempel:</span><span class="sxs-lookup"><span data-stu-id="122e6-135">For example:</span></span>

    Remove-AzureRmBatchAccount -AccountName <account_name>

<span data-ttu-id="122e6-136">När du uppmanas bekräfta tooremove hello-konto.</span><span class="sxs-lookup"><span data-stu-id="122e6-136">When prompted, confirm you want tooremove hello account.</span></span> <span data-ttu-id="122e6-137">Borttagning av konto kan ta viss tid toocomplete.</span><span class="sxs-lookup"><span data-stu-id="122e6-137">Account removal can take some time toocomplete.</span></span>

## <a name="create-a-batchaccountcontext-object"></a><span data-ttu-id="122e6-138">Skapa ett BatchAccountContext-objekt</span><span class="sxs-lookup"><span data-stu-id="122e6-138">Create a BatchAccountContext object</span></span>
<span data-ttu-id="122e6-139">Hej tooauthenticate med hjälp av Batch-PowerShell-cmdlets när du skapar och hanterar Batch pooler, projekt, uppgifter, och andra resurser, först skapa en BatchAccountContext objektet toostore ditt kontonamn och nycklar:</span><span class="sxs-lookup"><span data-stu-id="122e6-139">tooauthenticate using hello Batch PowerShell cmdlets when you create and manage Batch pools, jobs, tasks, and other resources, first create a BatchAccountContext object toostore your account name and keys:</span></span>

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

<span data-ttu-id="122e6-140">Du skickar hello BatchAccountContext objekt till cmdlets för att använda hello **BatchContext** parameter.</span><span class="sxs-lookup"><span data-stu-id="122e6-140">You pass hello BatchAccountContext object into cmdlets that use hello **BatchContext** parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="122e6-141">Som standard hello databaskontots primärnyckel används för autentisering, men du kan uttryckligen väljer hello viktiga toouse genom att ändra BatchAccountContext objektets **KeyInUse** egenskap: `$context.KeyInUse = "Secondary"`.</span><span class="sxs-lookup"><span data-stu-id="122e6-141">By default, hello account's primary key is used for authentication, but you can explicitly select hello key toouse by changing your BatchAccountContext object’s **KeyInUse** property: `$context.KeyInUse = "Secondary"`.</span></span>
> 
> 

## <a name="create-and-modify-batch-resources"></a><span data-ttu-id="122e6-142">Skapa och ändra Batch-resurser</span><span class="sxs-lookup"><span data-stu-id="122e6-142">Create and modify Batch resources</span></span>
<span data-ttu-id="122e6-143">Använd cmdlet: ar som **ny AzureBatchPool**, **ny AzureBatchJob**, och **ny AzureBatchTask** toocreate resurser under ett Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="122e6-143">Use cmdlets such as **New-AzureBatchPool**, **New-AzureBatchJob**, and **New-AzureBatchTask** toocreate resources under a Batch account.</span></span> <span data-ttu-id="122e6-144">Det motsvarande **Get -** och **Set -** cmdlets tooupdate hello egenskaperna för befintliga resurser och **Remove -** cmdlets tooremove resurser under ett Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="122e6-144">There are corresponding **Get-** and **Set-** cmdlets tooupdate hello properties of existing resources, and  **Remove-** cmdlets tooremove resources under a Batch account.</span></span>

<span data-ttu-id="122e6-145">När du använder många av dessa cmdlets i tillägg toopassing ett BatchContext-objekt, måste toocreate eller skicka objekt som innehåller detaljerade resursinställningar som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="122e6-145">When using many of these cmdlets, in addition toopassing a BatchContext object, you need toocreate or pass objects that contain detailed resource settings, as shown in hello following example.</span></span> <span data-ttu-id="122e6-146">Se hello utförlig information för varje cmdlet ytterligare exempel.</span><span class="sxs-lookup"><span data-stu-id="122e6-146">See hello detailed help for each cmdlet for additional examples.</span></span>

### <a name="create-a-batch-pool"></a><span data-ttu-id="122e6-147">Skapa en Batch-pool</span><span class="sxs-lookup"><span data-stu-id="122e6-147">Create a Batch pool</span></span>
<span data-ttu-id="122e6-148">När du skapar eller uppdaterar en Batch-pool du väljer hello molnet tjänstkonfiguration eller hello konfiguration av virtuell dator för hello operativsystemet på hello compute-noder (se [Batch funktionsöversikt](batch-api-basics.md#pool)).</span><span class="sxs-lookup"><span data-stu-id="122e6-148">When creating or updating a Batch pool, you select either hello cloud service configuration or hello virtual machine configuration for hello operating system on hello compute nodes (see [Batch feature overview](batch-api-basics.md#pool)).</span></span> <span data-ttu-id="122e6-149">Om du anger hello cloud service configuration compute-noder utrustas med en hello [Azure Gästoperativsystem släpper](../cloud-services/cloud-services-guestos-update-matrix.md#releases).</span><span class="sxs-lookup"><span data-stu-id="122e6-149">If you specify hello cloud service configuration, your compute nodes are imaged with one of hello [Azure Guest OS releases](../cloud-services/cloud-services-guestos-update-matrix.md#releases).</span></span> <span data-ttu-id="122e6-150">Om du anger hello konfiguration av virtuell dator kan du antingen ange en hello stöds Linux eller Windows VM bilder i listan hello [Azure virtuella datorer Marketplace][vm_marketplace], eller ange ett eget bilden som du har förberett.</span><span class="sxs-lookup"><span data-stu-id="122e6-150">If you specify hello virtual machine configuration, you can either specify one of hello supported Linux or Windows VM images listed in hello [Azure Virtual Machines Marketplace][vm_marketplace], or provide a custom image that you have prepared.</span></span>

<span data-ttu-id="122e6-151">När du kör **ny AzureBatchPool**, skickar hello operativsysteminställningar i ett PSCloudServiceConfiguration eller PSVirtualMachineConfiguration objekt.</span><span class="sxs-lookup"><span data-stu-id="122e6-151">When you run **New-AzureBatchPool**, pass hello operating system settings in a PSCloudServiceConfiguration or PSVirtualMachineConfiguration object.</span></span> <span data-ttu-id="122e6-152">Till exempel hello följande cmdlet skapar en ny Batch-pool med storleken små beräkningsnoder i hello cloud service configuration har avbildats med hello senaste operativsystemversion familj 3 (Windows Server 2012).</span><span class="sxs-lookup"><span data-stu-id="122e6-152">For example, hello following cmdlet creates a new Batch pool with size Small compute nodes in hello cloud service configuration, imaged with hello latest operating system version of family 3 (Windows Server 2012).</span></span> <span data-ttu-id="122e6-153">Här hello **CloudServiceConfiguration** parametern anger hello *$configuration* variabeln som hello PSCloudServiceConfiguration objekt.</span><span class="sxs-lookup"><span data-stu-id="122e6-153">Here, hello **CloudServiceConfiguration** parameter specifies hello *$configuration* variable as hello PSCloudServiceConfiguration object.</span></span> <span data-ttu-id="122e6-154">Hej **BatchContext** parametern anger en tidigare definierad variabel *$context* som hello BatchAccountContext objekt.</span><span class="sxs-lookup"><span data-stu-id="122e6-154">hello **BatchContext** parameter specifies a previously defined variable *$context* as hello BatchAccountContext object.</span></span>

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

<span data-ttu-id="122e6-155">hello mål antalet beräkningsnoder i hello ny pool bestäms av en autoskalning formel.</span><span class="sxs-lookup"><span data-stu-id="122e6-155">hello target number of compute nodes in hello new pool is determined by an autoscaling formula.</span></span> <span data-ttu-id="122e6-156">I det här fallet hello formeln är helt enkelt **$TargetDedicated = 4**, som visar hello antalet beräkningsnoder i poolen hello är 4.</span><span class="sxs-lookup"><span data-stu-id="122e6-156">In this case, hello formula is simply **$TargetDedicated=4**, indicating hello number of compute nodes in hello pool is 4 at most.</span></span>

## <a name="query-for-pools-jobs-tasks-and-other-details"></a><span data-ttu-id="122e6-157">Fråga avseende pooler, jobb, uppgifter och annan information</span><span class="sxs-lookup"><span data-stu-id="122e6-157">Query for pools, jobs, tasks, and other details</span></span>
<span data-ttu-id="122e6-158">Använd cmdlet: ar som **Get-AzureBatchPool**, **Get-AzureBatchJob**, och **Get-AzureBatchTask** tooquery för entiteter som skapats under ett Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="122e6-158">Use cmdlets such as **Get-AzureBatchPool**, **Get-AzureBatchJob**, and **Get-AzureBatchTask** tooquery for entities created under a Batch account.</span></span>

### <a name="query-for-data"></a><span data-ttu-id="122e6-159">Fråga efter data</span><span class="sxs-lookup"><span data-stu-id="122e6-159">Query for data</span></span>
<span data-ttu-id="122e6-160">Exempelvis använda **Get-AzureBatchPools** toofind din pooler.</span><span class="sxs-lookup"><span data-stu-id="122e6-160">As an example, use **Get-AzureBatchPools** toofind your pools.</span></span> <span data-ttu-id="122e6-161">Som standard detta frågor för alla pooler under kontot, under förutsättning att du redan lagras hello BatchAccountContext objekt i *$context*:</span><span class="sxs-lookup"><span data-stu-id="122e6-161">By default this queries for all pools under your account, assuming you already stored hello BatchAccountContext object in *$context*:</span></span>

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a><span data-ttu-id="122e6-162">Använda ett OData-filter</span><span class="sxs-lookup"><span data-stu-id="122e6-162">Use an OData filter</span></span>
<span data-ttu-id="122e6-163">Du kan ange en OData-filter som använder hello **Filter** parametern toofind hello endast objekt som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="122e6-163">You can supply an OData filter using hello **Filter** parameter toofind only hello objects you’re interested in.</span></span> <span data-ttu-id="122e6-164">Du kan till exempel hämta alla pooler med ID:n som börjar med ”myPool”:</span><span class="sxs-lookup"><span data-stu-id="122e6-164">For example, you can find all pools with ids starting with “myPool”:</span></span>

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

<span data-ttu-id="122e6-165">Den här metoden är inte lika flexibel som ”Where-Object”-metoden i en lokal pipeline.</span><span class="sxs-lookup"><span data-stu-id="122e6-165">This method is not as flexible as using “Where-Object” in a local pipeline.</span></span> <span data-ttu-id="122e6-166">Dock skickas hello fråga toohello Batch-tjänsten direkt så att alla filtrering uppstår på serversidan för hello, vilket sparar bandbredd för Internet.</span><span class="sxs-lookup"><span data-stu-id="122e6-166">However, hello query gets sent toohello Batch service directly so that all filtering happens on hello server side, saving Internet bandwidth.</span></span>

### <a name="use-hello-id-parameter"></a><span data-ttu-id="122e6-167">Använd hello Id-parametern</span><span class="sxs-lookup"><span data-stu-id="122e6-167">Use hello Id parameter</span></span>
<span data-ttu-id="122e6-168">Ett alternativt tooan OData-filtret är toouse hello **Id** parameter.</span><span class="sxs-lookup"><span data-stu-id="122e6-168">An alternative tooan OData filter is toouse hello **Id** parameter.</span></span> <span data-ttu-id="122e6-169">tooquery för en specifik grupp med id ”myPool”:</span><span class="sxs-lookup"><span data-stu-id="122e6-169">tooquery for a specific pool with id "myPool":</span></span>

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

<span data-ttu-id="122e6-170">Hej **Id** stöder endast fullständig-ID-sökning, inte jokertecken eller filter för OData-format.</span><span class="sxs-lookup"><span data-stu-id="122e6-170">hello **Id** parameter supports only full-id search, not wildcards or OData-style filters.</span></span>

### <a name="use-hello-maxcount-parameter"></a><span data-ttu-id="122e6-171">Använd hello MaxCount-parameter</span><span class="sxs-lookup"><span data-stu-id="122e6-171">Use hello MaxCount parameter</span></span>
<span data-ttu-id="122e6-172">Som standard returnerar varje cmdlet högst 1 000 objekt.</span><span class="sxs-lookup"><span data-stu-id="122e6-172">By default, each cmdlet returns a maximum of 1000 objects.</span></span> <span data-ttu-id="122e6-173">Om du når denna gräns och förfina din filter toobring tillbaka färre objekt, eller uttryckligen har angett maximalt med hello **MaxCount** parameter.</span><span class="sxs-lookup"><span data-stu-id="122e6-173">If you reach this limit, either refine your filter toobring back fewer objects, or explicitly set a maximum using hello **MaxCount** parameter.</span></span> <span data-ttu-id="122e6-174">Exempel:</span><span class="sxs-lookup"><span data-stu-id="122e6-174">For example:</span></span>

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

<span data-ttu-id="122e6-175">tooremove hello övre gränsen, ange **MaxCount** too0 eller mindre.</span><span class="sxs-lookup"><span data-stu-id="122e6-175">tooremove hello upper bound, set **MaxCount** too0 or less.</span></span>

### <a name="use-hello-powershell-pipeline"></a><span data-ttu-id="122e6-176">Använd hello PowerShell-pipeline</span><span class="sxs-lookup"><span data-stu-id="122e6-176">Use hello PowerShell pipeline</span></span>
<span data-ttu-id="122e6-177">Batch-cmdlets kan utnyttja hello PowerShell pipeline toosend data mellan cmdlets.</span><span class="sxs-lookup"><span data-stu-id="122e6-177">Batch cmdlets can leverage hello PowerShell pipeline toosend data between cmdlets.</span></span> <span data-ttu-id="122e6-178">Detta har samma effekt som anger en parameter, men gör att arbeta med flera enheter enklare hello.</span><span class="sxs-lookup"><span data-stu-id="122e6-178">This has hello same effect as specifying a parameter, but makes working with multiple entities easier.</span></span>

<span data-ttu-id="122e6-179">Exempelvis söka efter och visa alla aktiviteter på ditt konto:</span><span class="sxs-lookup"><span data-stu-id="122e6-179">For example, find and display all tasks under your account:</span></span>

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

<span data-ttu-id="122e6-180">Starta om varje beräkningsnod i en pool:</span><span class="sxs-lookup"><span data-stu-id="122e6-180">Restart (reboot) every compute node in a pool:</span></span>

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a><span data-ttu-id="122e6-181">Hantera programpaket</span><span class="sxs-lookup"><span data-stu-id="122e6-181">Application package management</span></span>
<span data-ttu-id="122e6-182">Programpaket erbjuder ett förenklat toodeploy program toohello compute-noder i din pooler.</span><span class="sxs-lookup"><span data-stu-id="122e6-182">Application packages provide a simplified way toodeploy applications toohello compute nodes in your pools.</span></span> <span data-ttu-id="122e6-183">Du kan använda hello Batch PowerShell-cmdlets för att ladda upp och hantera programpaket i Batch-kontot och distribuera paketet versioner toocompute noder.</span><span class="sxs-lookup"><span data-stu-id="122e6-183">With hello Batch PowerShell cmdlets, you can upload and manage application packages in your Batch account, and deploy package versions toocompute nodes.</span></span>

<span data-ttu-id="122e6-184">**Skapa** ett program:</span><span class="sxs-lookup"><span data-stu-id="122e6-184">**Create** an application:</span></span>

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

<span data-ttu-id="122e6-185">**Lägg till** ett programpaket:</span><span class="sxs-lookup"><span data-stu-id="122e6-185">**Add** an application package:</span></span>

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

<span data-ttu-id="122e6-186">Ange hello **standardversionen** för programmet hello:</span><span class="sxs-lookup"><span data-stu-id="122e6-186">Set hello **default version** for hello application:</span></span>

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

<span data-ttu-id="122e6-187">**Lista** ett programs paket</span><span class="sxs-lookup"><span data-stu-id="122e6-187">**List** an application's packages</span></span>

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

<span data-ttu-id="122e6-188">**Ta bort** ett programpaket</span><span class="sxs-lookup"><span data-stu-id="122e6-188">**Delete** an application package</span></span>

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

<span data-ttu-id="122e6-189">**Ta bort** ett program</span><span class="sxs-lookup"><span data-stu-id="122e6-189">**Delete** an application</span></span>

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

> [!NOTE]
> <span data-ttu-id="122e6-190">Du måste ta bort alla programpaketversioner för ett program innan du tar bort hello program.</span><span class="sxs-lookup"><span data-stu-id="122e6-190">You must delete all of an application's application package versions before you delete hello application.</span></span> <span data-ttu-id="122e6-191">Kommer felmeddelandet ”konflikt' om du försöker toodelete ett program som för närvarande har programpaket.</span><span class="sxs-lookup"><span data-stu-id="122e6-191">You will receive a 'Conflict' error if you try toodelete an application that currently has application packages.</span></span>
> 
> 

### <a name="deploy-an-application-package"></a><span data-ttu-id="122e6-192">Distribuera ett programpaket</span><span class="sxs-lookup"><span data-stu-id="122e6-192">Deploy an application package</span></span>
<span data-ttu-id="122e6-193">Du kan ange ett eller flera programpaket för distribution när du skapar en pool.</span><span class="sxs-lookup"><span data-stu-id="122e6-193">You can specify one or more application packages for deployment when you create a pool.</span></span> <span data-ttu-id="122e6-194">När du anger ett paket vid tidpunkten för skapandet av poolen är distribuerade tooeach nod som hello nod ansluter till poolen.</span><span class="sxs-lookup"><span data-stu-id="122e6-194">When you specify a package at pool creation time, it is deployed tooeach node as hello node joins pool.</span></span> <span data-ttu-id="122e6-195">Paket distribueras också när en nod startas om eller när en avbildning återställs.</span><span class="sxs-lookup"><span data-stu-id="122e6-195">Packages are also deployed when a node is rebooted or reimaged.</span></span>

<span data-ttu-id="122e6-196">Ange hello `-ApplicationPackageReference` alternativ när du skapar en pool toodeploy paketet toohello programpoolens noder som de gå hello poolen.</span><span class="sxs-lookup"><span data-stu-id="122e6-196">Specify hello `-ApplicationPackageReference` option when creating a pool toodeploy an application package toohello pool's nodes as they join hello pool.</span></span> <span data-ttu-id="122e6-197">Skapa först en **PSApplicationPackageReference** objekt och konfigurera det med hello program-Id och paketet version du vill använda toodeploy toohello pool compute-noder:</span><span class="sxs-lookup"><span data-stu-id="122e6-197">First, create a **PSApplicationPackageReference** object, and configure it with hello application Id and package version you want toodeploy toohello pool's compute nodes:</span></span>

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

<span data-ttu-id="122e6-198">Nu skapar hello pool, och ange hello paketet referensobjekt som hello argumentet toohello `ApplicationPackageReferences` alternativ:</span><span class="sxs-lookup"><span data-stu-id="122e6-198">Now create hello pool, and specify hello package reference object as hello argument toohello `ApplicationPackageReferences` option:</span></span>

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

<span data-ttu-id="122e6-199">Du hittar mer information om programpaket i [distribuera program toocompute noder med Batch-programpaket](batch-application-packages.md).</span><span class="sxs-lookup"><span data-stu-id="122e6-199">You can find more information on application packages in [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="122e6-200">Du måste [länka ett Azure Storage-konto](#linked-storage-account-autostorage) tooyour Batch-kontot toouse programpaket.</span><span class="sxs-lookup"><span data-stu-id="122e6-200">You must [link an Azure Storage account](#linked-storage-account-autostorage) tooyour Batch account toouse application packages.</span></span>
> 
> 

### <a name="update-a-pools-application-packages"></a><span data-ttu-id="122e6-201">Uppdatera programpaket för en pool</span><span class="sxs-lookup"><span data-stu-id="122e6-201">Update a pool's application packages</span></span>
<span data-ttu-id="122e6-202">tooupdate hello program tilldelade tooan befintlig pool, först skapa ett PSApplicationPackageReference-objekt med hello önskade egenskaper (program-Id och paketet version):</span><span class="sxs-lookup"><span data-stu-id="122e6-202">tooupdate hello applications assigned tooan existing pool, first create a PSApplicationPackageReference object with hello desired properties (application Id and package version):</span></span>

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

<span data-ttu-id="122e6-203">Därefter hämta hello pool från Batch, rensa eventuella befintliga paket, Lägg till vår nya paket referens och uppdatera hello Batch-tjänsten med hello nya inställningar för programpool:</span><span class="sxs-lookup"><span data-stu-id="122e6-203">Next, get hello pool from Batch, clear out any existing packages, add our new package reference, and update hello Batch service with hello new pool settings:</span></span>

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

<span data-ttu-id="122e6-204">Hello programpoolens egenskaper i hello Batch-tjänsten har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="122e6-204">You've now updated hello pool's properties in hello Batch service.</span></span> <span data-ttu-id="122e6-205">tooactually distribuera hello nya program paketet toocompute noder i hello pool, men du måste starta om eller återavbilda noderna.</span><span class="sxs-lookup"><span data-stu-id="122e6-205">tooactually deploy hello new application package toocompute nodes in hello pool, however, you must restart or reimage those nodes.</span></span> <span data-ttu-id="122e6-206">Du kan starta om varje nod i en pool med det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="122e6-206">You can restart every node in a pool with this command:</span></span>

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

> [!TIP]
> <span data-ttu-id="122e6-207">Du kan distribuera flera program paket toohello compute-noder i en pool.</span><span class="sxs-lookup"><span data-stu-id="122e6-207">You can deploy multiple application packages toohello compute nodes in a pool.</span></span> <span data-ttu-id="122e6-208">Om du vill ha för*lägga till* ett programpaket i stället för att ersätta hello som för närvarande har distribuerats paket utelämna hello `$pool.ApplicationPackageReferences.Clear()` raden ovan.</span><span class="sxs-lookup"><span data-stu-id="122e6-208">If you'd like too*add* an application package instead of replacing hello currently deployed packages, omit hello `$pool.ApplicationPackageReferences.Clear()` line above.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="122e6-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="122e6-209">Next steps</span></span>
* <span data-ttu-id="122e6-210">Detaljerad cmdlet-syntax och exempel finns i [Cmdlet-referens för Azure Batch](/powershell/module/azurerm.batch/#batch).</span><span class="sxs-lookup"><span data-stu-id="122e6-210">For detailed cmdlet syntax and examples, see [Azure Batch cmdlet reference](/powershell/module/azurerm.batch/#batch).</span></span>
* <span data-ttu-id="122e6-211">Mer information om program och programpaket i Batch finns [distribuera program toocompute noder med Batch-programpaket](batch-application-packages.md).</span><span class="sxs-lookup"><span data-stu-id="122e6-211">For more information about applications and application packages in Batch, see [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md).</span></span>

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/