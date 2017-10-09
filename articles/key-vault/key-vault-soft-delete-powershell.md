---
ms.assetid: 
title: "aaaAzure nyckel valvet – hur toouse mjuk borttagning med PowerShell"
description: "Använda case exempel på mjuk borttagning med PowerShell-kodstyckena"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bruceper
ms.openlocfilehash: 4968b700a14f764ea1be7de2bf3697664f255f95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-key-vault-soft-delete-with-powershell"></a><span data-ttu-id="9a4f5-103">Hur toouse Key Vault mjuk borttagning med PowerShell</span><span class="sxs-lookup"><span data-stu-id="9a4f5-103">How toouse Key Vault soft-delete with PowerShell</span></span>

<span data-ttu-id="9a4f5-104">Funktionen för mjuk borttagning av Azure Key Vault kan återställning av borttagna valv och valvet objekt.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-104">Azure Key Vault's soft delete feature allows recovery of deleted vaults and vault objects.</span></span> <span data-ttu-id="9a4f5-105">Mer specifikt mjuk borttagning adresser hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-105">Specifically, soft-delete addresses hello following scenarios:</span></span>

- <span data-ttu-id="9a4f5-106">Stöd för återställningsbara borttagningen av ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="9a4f5-106">Support for recoverable deletion of a key vault</span></span>
- <span data-ttu-id="9a4f5-107">Stöd för återställningsbara borttagning av objekt i nyckelvalvet. nycklar hemligheter, och certifikat</span><span class="sxs-lookup"><span data-stu-id="9a4f5-107">Support for recoverable deletion of key vault objects; keys, secrets, and, certificates</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a4f5-108">Krav</span><span class="sxs-lookup"><span data-stu-id="9a4f5-108">Prerequisites</span></span>

- <span data-ttu-id="9a4f5-109">Azure PowerShell 4.0.0 eller senare - om du inte redan har gjort det här installationsprogrammet, installera Azure PowerShell och koppla den till din Azure-prenumeration, se [hur tooinstall och konfigurera Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9a4f5-109">Azure PowerShell 4.0.0 or later - If you don't have this already setup, install Azure PowerShell and associate it with your Azure subscription, see [How tooinstall and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span> 

>[!NOTE]
> <span data-ttu-id="9a4f5-110">Det finns en inaktuell version av våra Key Vault PowerShell-utdata formatering fil som **kan** läsas in i din miljö i stället för hello rätt version.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-110">There is an outdated version of our Key Vault PowerShell output formatting file that **may** be loaded into your environment instead of hello correct version.</span></span> <span data-ttu-id="9a4f5-111">Vi förutse en uppdaterad version av PowerShell toocontain hello behövs korrigering för hello utdata formatering och kommer att uppdatera det här avsnittet som för närvarande.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-111">We are anticipating an updated version of PowerShell toocontain hello needed correction for hello output formatting and will update this topic at that time.</span></span> <span data-ttu-id="9a4f5-112">Hej aktuella lösning bör du stöta på problemet formatering är:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-112">hello current workaround, should you encounter this formatting problem, is:</span></span>
> - <span data-ttu-id="9a4f5-113">Använd hello följande fråga om du upptäcker att det inte uppstår hello mjuk borttagning enabled-egenskap som beskrivs i det här avsnittet: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-113">Use hello following query if you notice you're not seeing hello soft-delete enabled property described in this topic: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span></span>


<span data-ttu-id="9a4f5-114">Key Vault specifika refernece information för PowerShell finns i [Azure Key Vault PowerShell-referens](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span><span class="sxs-lookup"><span data-stu-id="9a4f5-114">For Key Vault specific refernece information for PowerShell, see [Azure Key Vault PowerShell reference](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span></span>

## <a name="required-permissions"></a><span data-ttu-id="9a4f5-115">Behörigheter som krävs</span><span class="sxs-lookup"><span data-stu-id="9a4f5-115">Required permissions</span></span>

<span data-ttu-id="9a4f5-116">Key Vault hanteras separat via rollbaserade behörigheter för åtkomstkontroll (RBAC) enligt följande:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-116">Key Vault operations are separately managed via role-based access control (RBAC) permissions as follows:</span></span>

| <span data-ttu-id="9a4f5-117">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="9a4f5-117">Operation</span></span> | <span data-ttu-id="9a4f5-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9a4f5-118">Description</span></span> | <span data-ttu-id="9a4f5-119">Användarbehörighet</span><span class="sxs-lookup"><span data-stu-id="9a4f5-119">User permission</span></span> |
|:--|:--|:--|
|<span data-ttu-id="9a4f5-120">Visa lista</span><span class="sxs-lookup"><span data-stu-id="9a4f5-120">List</span></span>|<span data-ttu-id="9a4f5-121">Visar bort nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-121">Lists deleted key vaults.</span></span>|<span data-ttu-id="9a4f5-122">Microsoft.KeyVault/deletedVaults/read</span><span class="sxs-lookup"><span data-stu-id="9a4f5-122">Microsoft.KeyVault/deletedVaults/read</span></span>|
|<span data-ttu-id="9a4f5-123">Återställ</span><span class="sxs-lookup"><span data-stu-id="9a4f5-123">Recover</span></span>|<span data-ttu-id="9a4f5-124">Återställer borttagna nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-124">Restores a deleted key vault.</span></span>|<span data-ttu-id="9a4f5-125">Microsoft.KeyVault/vaults/write</span><span class="sxs-lookup"><span data-stu-id="9a4f5-125">Microsoft.KeyVault/vaults/write</span></span>|
|<span data-ttu-id="9a4f5-126">Rensa</span><span class="sxs-lookup"><span data-stu-id="9a4f5-126">Purge</span></span>|<span data-ttu-id="9a4f5-127">Tar permanent bort borttagna nyckelvalvet och allt dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-127">Permanently removes a deleted key vault and all its contents.</span></span>|<span data-ttu-id="9a4f5-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span><span class="sxs-lookup"><span data-stu-id="9a4f5-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span></span>|

<span data-ttu-id="9a4f5-129">Mer information om behörighet och åtkomstkontroll finns [Secure nyckelvalvet](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="9a4f5-129">For more information on permissions and access control, see [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>

## <a name="enabling-soft-delete"></a><span data-ttu-id="9a4f5-130">Aktivera mjuk borttagning</span><span class="sxs-lookup"><span data-stu-id="9a4f5-130">Enabling soft-delete</span></span>

<span data-ttu-id="9a4f5-131">toobe kan toorecover borttagna nyckelvalv eller objekt som lagras i en nyckel valvet, måste du först aktivera mjuk borttagning för att nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-131">toobe able toorecover a deleted key vault or objects stored in a key vault, you must first enable soft-delete for that key vault.</span></span>

### <a name="existing-key-vault"></a><span data-ttu-id="9a4f5-132">Befintliga nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="9a4f5-132">Existing key vault</span></span>

<span data-ttu-id="9a4f5-133">Aktivera mjuk borttagning på följande sätt för en befintlig nyckelvalv med namnet ContosoVault.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-133">For an existing key vault named ContosoVault, enable soft-delete as follows.</span></span> 

>[!NOTE]
><span data-ttu-id="9a4f5-134">För närvarande måste toouse Azure Resource Manager resurs manipulering toodirectly skrivåtgärder hello *enableSoftDelete* egenskapen toohello Key Vault-resurs.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-134">Currently you need toouse Azure Resource Manager resource manipulation toodirectly write hello *enableSoftDelete* property toohello Key Vault resource.</span></span>

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a><span data-ttu-id="9a4f5-135">Nytt nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="9a4f5-135">New key vault</span></span>

<span data-ttu-id="9a4f5-136">Aktivera mjuk borttagning för ett nytt nyckelvalv görs vid skapandet genom att lägga till hello soft-ta bort flaggan tooyour skapa kommando.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-136">Enabling soft-delete for a new key vault is done at creation time by adding hello soft-delete enable flag tooyour create command.</span></span>

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a><span data-ttu-id="9a4f5-137">Kontrollera mjuk borttagning aktivering</span><span class="sxs-lookup"><span data-stu-id="9a4f5-137">Verify soft-delete enablement</span></span>

<span data-ttu-id="9a4f5-138">tooverify som ett nyckelvalv har aktiverat mjuk borttagning, kör hello *hämta* kommandot och leta efter hello ”ej permanent ta bort aktiverat'</span><span class="sxs-lookup"><span data-stu-id="9a4f5-138">tooverify that a key vault has soft-delete enabled, run hello *get* command and look for hello 'Soft Delete Enabled?'</span></span> <span data-ttu-id="9a4f5-139">attribut och dess inställning, true eller false.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-139">attribute and its setting, true or false.</span></span>

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a><span data-ttu-id="9a4f5-140">Om du tar bort ett nyckelvalv som skyddas av mjuk borttagning</span><span class="sxs-lookup"><span data-stu-id="9a4f5-140">Deleting a key vault protected by soft-delete</span></span>

<span data-ttu-id="9a4f5-141">hello kommandot toodelete (eller ta bort) en nyckelvalv förblir hello detsamma, men dess funktionsändringar beroende på om du har aktiverat soft-ta bort eller inte.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-141">hello command toodelete (or remove) a key vault remains hello same, but its behavior changes depending on whether you have enabled soft-delete or not.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
><span data-ttu-id="9a4f5-142">Om du kör hello föregående kommando för ett nyckelvalv som inte har aktiverat soft-delete tar permanent bort den här nyckelvalvet och allt dess innehåll utan alternativ för återställning.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-142">If you run hello previous command for a key vault that does not have soft-delete enabled, you will permanently delete this key vault and all its content without any options for recovery.</span></span>

### <a name="how-soft-delete-protects-your-key-vaults"></a><span data-ttu-id="9a4f5-143">Hur mjuk borttagning skyddar ditt nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="9a4f5-143">How soft-delete protects your key vaults</span></span>

<span data-ttu-id="9a4f5-144">Aktiverad med mjuk borttagning:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-144">With soft-delete enabled:</span></span>

- <span data-ttu-id="9a4f5-145">När en nyckelvalv tas bort, den tas bort från dess resursgrupp och placeras i ett reserverat namnområde som endast är associerad med hello plats där den skapades.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-145">When a key vault is deleted, it is removed from its resource group and placed in a reserved namespace that is only associated with hello location where it was created.</span></span> 
- <span data-ttu-id="9a4f5-146">Objekt i en borttagen för valvet, t.ex. nycklar, hemligheter och certifikat, är tillgänglig och förblir det medan sina som innehåller nyckelvalv hello bort tillstånd.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-146">Objects in a deleted key vault, such as keys, secrets and, certificates, are inaccessible and remain so while their containing key vault is in hello deleted state.</span></span> 
- <span data-ttu-id="9a4f5-147">hello DNS-namn för nyckelvalvet i ett borttaget tillstånd är kvar så det inte går att skapa ett nytt nyckelvalv med samma namn.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-147">hello DNS name for a key vault in a deleted state is still reserved so, a new key vault with same name cannot be created.</span></span>  

<span data-ttu-id="9a4f5-148">Du kan visa Borttaget tillstånd nyckelvalv, som är associerade med din prenumeration med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-148">You may view deleted state key vaults, associated with your subscription, using hello following command:</span></span>

```powershell
PS C:\> Get-AzureRmKeyVault -InRemovedStateVault 

Name           : ContosoVault
Location             : westus
Id                   : /subscriptions/xxx/providers/Microsoft.KeyVault/locations/westus/deletedVaults/ContosoVault
Resource ID          : /subscriptions/xxx/resourceGroups/ContosoVault/providers/Microsoft.KeyVault/vaults/ContosoVault
Deletion Date        : 5/9/2017 12:14:14 AM
Scheduled Purge Date : 8/7/2017 12:14:14 AM
Tags                 :
```

<span data-ttu-id="9a4f5-149">Hej *resurs-ID* i hello utdata refererar toohello ursprungliga resurs-ID för det här valvet.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-149">hello *Resource ID* in hello output refers toohello original resource ID of this vault.</span></span> <span data-ttu-id="9a4f5-150">Eftersom den här nyckelvalv är nu i ett borttaget tillstånd, det finns någon resurs med resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-150">Since this key vault is now in a deleted state, no resource exists with that resource ID.</span></span> <span data-ttu-id="9a4f5-151">Hej *Id* fältet kan vara används tooidentify hello resurs när du återställer eller rensa.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-151">hello *Id* field can be used tooidentify hello resource when recovering, or purging.</span></span> <span data-ttu-id="9a4f5-152">Hej *schemalagda Rensa datum* anger när hello valvet tas bort permanent (rensas) om ingen åtgärd utförs för borttagna valvet.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-152">hello *Scheduled Purge Date* field indicates when hello vault will be permanently deleted (purged) if no action is taken for this deleted vault.</span></span> <span data-ttu-id="9a4f5-153">hello standard kvarhållning period, använt toocalculate hello *schemalagda Rensa datum*, är 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-153">hello default retention period, used toocalculate hello *Scheduled Purge Date*, is 90 days.</span></span>

## <a name="recovering-a-key-vault"></a><span data-ttu-id="9a4f5-154">Återställa en nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="9a4f5-154">Recovering a key vault</span></span>

<span data-ttu-id="9a4f5-155">toorecover ett nyckelvalv måste toospecify hello nyckelvalv namn, resursgruppens namn och plats.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-155">toorecover a key vault, you need toospecify hello key vault name, resource group, and location.</span></span> <span data-ttu-id="9a4f5-156">Obs hello plats och hello resursgruppen av hello bort nyckelvalv som du behöver dem för en nyckelvalv återställningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-156">Note hello location and hello resource group of hello deleted key vault as you need these for a key vault recovery process.</span></span>

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

<span data-ttu-id="9a4f5-157">När en nyckelvalv återställs är hello resultatet en ny resurs med hello nyckelvalv ursprungliga resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-157">When a key vault is recovered, hello result is a new resource with hello key vault's original resource ID.</span></span> <span data-ttu-id="9a4f5-158">Om hello resursgruppen där hello nyckelvalv fanns har tagits bort, måste en ny resursgrupp med samma namn skapas innan hello nyckelvalv kan återställas.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-158">If hello resource group where hello key vault existed has been removed, a new resource group with same name must be created before hello key vault can be recovered.</span></span>

## <a name="key-vault-objects-and-soft-delete"></a><span data-ttu-id="9a4f5-159">Key Vault-objekt och Mjuk borttagning</span><span class="sxs-lookup"><span data-stu-id="9a4f5-159">Key Vault objects and soft-delete</span></span>

<span data-ttu-id="9a4f5-160">För en nyckel ContosoFirstKey, i en källnyckelvalvets med namnet 'ContosoVault' med mjuk borttagning aktiverad här hur du skulle ta bort nyckeln.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-160">For a key, 'ContosoFirstKey', in a key vault named 'ContosoVault' with soft-delete enabled, here's how you would delete that key.</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="9a4f5-161">Med nyckelvalvet aktiverad för mjuk borttagning, visas borttagna nyckeln fortfarande som om det har tagits bort utom, när du inte uttryckligen lista eller hämta borttagna nycklar.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-161">With your key vault enabled for soft-delete, a deleted key still appears like it's deleted except, when you explicitly list or retrieve deleted keys.</span></span> <span data-ttu-id="9a4f5-162">De flesta åtgärder på en nyckel i hello bort tillstånd misslyckas förutom visar en lista över borttagna nyckeln, återställs eller rensa den.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-162">Most operations on a key in hello deleted state will fail except for listing a deleted key, recovering it or purging it.</span></span> 

<span data-ttu-id="9a4f5-163">Till exempel använda toorequest toolist bort nycklar i nyckelvalvet, hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-163">For example, toorequest toolist deleted keys in a key vault, use hello following command:</span></span>

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a><span data-ttu-id="9a4f5-164">Övergångstillstånd</span><span class="sxs-lookup"><span data-stu-id="9a4f5-164">Transition state</span></span> 

<span data-ttu-id="9a4f5-165">Det kan ta några sekunder för hello övergången toocomplete när du tar bort en nyckel i ett nyckelvalv med aktiverad mjuk borttagning.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-165">When you delete a key in a key vault with soft-delete enabled, it may take a few seconds for hello transition toocomplete.</span></span> <span data-ttu-id="9a4f5-166">Under den här övergångstillstånd kan visas hello nyckeln inte är i hello active eller hello bort tillstånd.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-166">During this transition state, it may appear that hello key is not in hello active state or hello deleted state.</span></span> <span data-ttu-id="9a4f5-167">Det här kommandot visar en lista över alla borttagna nycklar i nyckelvalvet med namnet 'ContosoVault'.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-167">This command will list all deleted keys in your key vault named 'ContosoVault'.</span></span>

```powershell
  Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
  Vault Name           : ContosoVault
  Name                 : ContosoFirstKey
  Id                   : https://ContosoVault.vault.azure.net:443/keys/ContosoFirstKey
  Deleted Date         : 2/14/2017 8:20:52 PM
  Scheduled Purge Date : 5/15/2017 8:20:52 PM
  Enabled              : True
  Expires              :
  Not Before           :
  Created              : 2/14/2017 8:16:07 PM
  Updated              : 2/14/2017 8:16:07 PM
  Tags                 :
```

### <a name="using-soft-delete-with-key-vault-objects"></a><span data-ttu-id="9a4f5-168">Hur mjuk borttagning med nyckelvalv objekt</span><span class="sxs-lookup"><span data-stu-id="9a4f5-168">Using soft-delete with key vault objects</span></span>

<span data-ttu-id="9a4f5-169">Precis som nyckelvalv, en borttagen nyckel hemlighet eller certifikat finns kvar i Borttaget tillstånd för in too90 dagar såvida du inte återställa den eller rensa den.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-169">Just like key vaults, a deleted key, secret or, certificate will remain in deleted state for up too90 days unless you recover it or purge it.</span></span> 

#### <a name="keys"></a><span data-ttu-id="9a4f5-170">Nycklar</span><span class="sxs-lookup"><span data-stu-id="9a4f5-170">Keys</span></span>

<span data-ttu-id="9a4f5-171">toorecover borttagna nyckeln:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-171">toorecover a deleted key:</span></span>

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="9a4f5-172">toopermanently ta bort en nyckel:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-172">toopermanently delete a key:</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
><span data-ttu-id="9a4f5-173">Rensa en nyckel tar permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-173">Purging a key will permanently delete it, meaning it will not be recoverable.</span></span>

<span data-ttu-id="9a4f5-174">Hej **återställa** och **Rensa** åtgärder har sina egna behörigheter i en åtkomstprincip för nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-174">hello **recover** and **purge** actions have their own permissions associated in a key vault access policy.</span></span> <span data-ttu-id="9a4f5-175">För en användare eller tjänst huvudnamn toobe kan tooexecute en **återställa** eller **Rensa** åtgärd som de måste ha hello respektive behörighet för objektet (nyckel eller hemlighet) i hello nyckelvalv åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-175">For a user or service principal toobe able tooexecute a **recover** or **purge** action they must have hello respective permission for that object (key or secret) in hello key vault access policy.</span></span> <span data-ttu-id="9a4f5-176">Som standard hello **Rensa** behörighet läggs inte tooa nyckelvalv åtkomstprincip hello 'all' genväg kan du använda toogrant alla behörigheter tooa användare.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-176">By default, hello **purge** permission is not added tooa key vault's access policy when hello 'all' shortcut is used toogrant all permissions tooa user.</span></span> <span data-ttu-id="9a4f5-177">Du måste uttryckligen bevilja **Rensa** behörighet.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-177">You must explicitly grant **purge** permission.</span></span> <span data-ttu-id="9a4f5-178">Till exempel hello efter kommandot ger user@contoso.com behörighet tooperform flera åtgärder med nycklar i *ContosoVault* inklusive **Rensa**.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-178">For example, hello following command grants user@contoso.com permission tooperform several operations on keys in *ContosoVault* including **purge**.</span></span>

#### <a name="set-a-key-vault-access-policy"></a><span data-ttu-id="9a4f5-179">Skapa en princip för nyckelvalvet åtkomst</span><span class="sxs-lookup"><span data-stu-id="9a4f5-179">Set a key vault access policy</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> <span data-ttu-id="9a4f5-180">Om du har en befintlig nyckelvalv som precis har haft soft-ta bort aktiverat kan du kanske inte har **återställa** och **Rensa** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-180">If you have an existing key vault that has just had soft-delete enabled, you may not have **recover** and **purge** permissions.</span></span>

#### <a name="secrets"></a><span data-ttu-id="9a4f5-181">Hemligheter</span><span class="sxs-lookup"><span data-stu-id="9a4f5-181">Secrets</span></span>

<span data-ttu-id="9a4f5-182">Som nycklar drivs hemligheter i en nyckelvalvet på med sina egna kommandon.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-182">Like keys, secrets in a key vault are operated on with their own commands.</span></span> <span data-ttu-id="9a4f5-183">Följande, är hello-kommandon för att ta bort, lista, återställa och rensa hemligheter.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-183">Following, are hello commands for deleting, listing, recovering, and purging secrets.</span></span>

- <span data-ttu-id="9a4f5-184">Ta bort en hemlighet som heter SQLPassword:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-184">Delete a secret named SQLPassword:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- <span data-ttu-id="9a4f5-185">Lista alla borttagna hemligheter i en nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-185">List all deleted secrets in a key vault:</span></span> 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- <span data-ttu-id="9a4f5-186">Återställa en hemlighet i hello bort tillstånd:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-186">Recover a secret in hello deleted state:</span></span> 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- <span data-ttu-id="9a4f5-187">Rensa en hemlighet i Borttaget tillstånd:</span><span class="sxs-lookup"><span data-stu-id="9a4f5-187">Purge a secret in deleted state:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
><span data-ttu-id="9a4f5-188">Rensa en hemlighet tar permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-188">Purging a secret will permanently delete it, meaning it will not be recoverable.</span></span>

## <a name="purging-and-key-vaults"></a><span data-ttu-id="9a4f5-189">Ovanstående och nyckel valv</span><span class="sxs-lookup"><span data-stu-id="9a4f5-189">Purging and key vaults</span></span>

### <a name="key-vault-objects"></a><span data-ttu-id="9a4f5-190">Nyckelvalv objekt</span><span class="sxs-lookup"><span data-stu-id="9a4f5-190">Key vault objects</span></span>

<span data-ttu-id="9a4f5-191">Rensa en nyckel tar hemlighet eller certifikat permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-191">Purging a key, secret or, certificate will permanently delete it, meaning it will not be recoverable.</span></span> <span data-ttu-id="9a4f5-192">Hej nyckelvalv med hello borttagna objekt förblir men intakta som kommer alla andra objekt i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-192">hello key vault that contained hello deleted object will however remain intact as will all other objects in hello key vault.</span></span> 

### <a name="key-vaults-as-containers"></a><span data-ttu-id="9a4f5-193">Nyckel-valv som behållare</span><span class="sxs-lookup"><span data-stu-id="9a4f5-193">Key vaults as containers</span></span>
<span data-ttu-id="9a4f5-194">När en nyckelvalv rensas tas allt innehåll, inklusive nycklar och hemligheter certifikat bort permanent.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-194">When a key vault is purged, all of its contents, including keys, secrets, and certificates, are permanently deleted.</span></span> <span data-ttu-id="9a4f5-195">toopurge nyckelvalvet, använda hello `Remove-AzureRmKeyVault` med hello alternativet `-InRemovedState` och genom att ange hello platsen för hello bort nyckelvalv med hello `-Location location` argumentet.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-195">toopurge a key vault, use hello `Remove-AzureRmKeyVault` command with hello option `-InRemovedState` and by specifying hello location of hello deleted key vault with hello `-Location location` argument.</span></span> <span data-ttu-id="9a4f5-196">Du kan hitta hello platsen för ett borttaget valv hello kommandot `Get-AzureRmKeyVault -InRemovedState`.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-196">You can find hello location of a deleted vault using hello command `Get-AzureRmKeyVault -InRemovedState`.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
><span data-ttu-id="9a4f5-197">Rensa en key vault tar permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-197">Purging a key vault will permanently delete it, meaning it will not be recoverable.</span></span>

### <a name="purge-permissions-required"></a><span data-ttu-id="9a4f5-198">Rensa behörigheter som krävs</span><span class="sxs-lookup"><span data-stu-id="9a4f5-198">Purge permissions required</span></span>
- <span data-ttu-id="9a4f5-199">toopurge borttagna nyckelvalvet, så att hello valvet och allt dess innehåll tas bort permanent, hello användare behöver RBAC behörighet tooperform en *Microsoft.KeyVault/locations/deletedVaults/purge/action* igen.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-199">toopurge a deleted key vault, such that hello vault and all its contents are permanently removed, hello user needs RBAC permission tooperform a *Microsoft.KeyVault/locations/deletedVaults/purge/action* operation.</span></span> 
- <span data-ttu-id="9a4f5-200">toolist hello bort nyckel hello valvet en användare behöver RBAC behörighet tooperform *Microsoft.KeyVault/deletedVaults/read* behörighet.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-200">toolist hello deleted key, hello vault a user needs RBAC permission tooperform *Microsoft.KeyVault/deletedVaults/read* permission.</span></span> 
- <span data-ttu-id="9a4f5-201">Som standard har bara en prenumeration administratör dessa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-201">By default only a subscription administrator has these permissions.</span></span> 

### <a name="scheduled-purge"></a><span data-ttu-id="9a4f5-202">Schemalagda Rensa</span><span class="sxs-lookup"><span data-stu-id="9a4f5-202">Scheduled purge</span></span>

<span data-ttu-id="9a4f5-203">Visar en lista över borttagna nyckelvalv-objekt visar när de är schedled toobe rensas av Key Vault.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-203">Listing your deleted key vault objects shows when they are schedled toobe purged by Key Vault.</span></span> <span data-ttu-id="9a4f5-204">Hej *schemalagda Rensa datum* fältet visas när ett nyckelvalv objekt tas bort permanent, om ingen åtgärd utförs.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-204">hello *Scheduled Purge Date* field indicates when a key vault object will be permanently deleted, if no action is taken.</span></span> <span data-ttu-id="9a4f5-205">Som standard är hello kvarhållningsperiod för ett objekt för borttagna nyckelvalv 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-205">By default, hello retention period for a deleted key vault object is 90 days.</span></span>

>[!NOTE]
><span data-ttu-id="9a4f5-206">Ett objekt för borttagna valvet utlöses av dess *schemalagda Rensa datum* fältet, bort permanent.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-206">A purged vault object, triggered by its *Scheduled Purge Date* field, is permanently deleted.</span></span> <span data-ttu-id="9a4f5-207">Den kan inte återställas.</span><span class="sxs-lookup"><span data-stu-id="9a4f5-207">It is not recoverable.</span></span>

## <a name="other-resources"></a><span data-ttu-id="9a4f5-208">Andra resurser</span><span class="sxs-lookup"><span data-stu-id="9a4f5-208">Other resources</span></span>

- <span data-ttu-id="9a4f5-209">En översikt över funktionen för mjuk borttagning av Key Vault finns [översikt över Azure Key Vault-soft-ta bort](key-vault-ovw-soft-delete.md).</span><span class="sxs-lookup"><span data-stu-id="9a4f5-209">For an overview of Key Vault's soft-delete feature, see [Azure Key Vault soft-delete overview](key-vault-ovw-soft-delete.md).</span></span>
- <span data-ttu-id="9a4f5-210">En allmän översikt över användning av Azure Key Vault finns [Kom igång med Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9a4f5-210">For a general overview of Azure Key Vault usage, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>

