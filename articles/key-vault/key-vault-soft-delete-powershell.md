---
ms.assetid: 
title: "Azure Key Vault - hur du använder mjuk borttagning med PowerShell"
description: "Använda case exempel på mjuk borttagning med PowerShell-kodstyckena"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bruceper
ms.openlocfilehash: 8cf0674f7eb139e50da4a3c22a8d8376a86b0dcc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a><span data-ttu-id="7b492-103">Hur du använder Key Vault mjuk borttagning med PowerShell</span><span class="sxs-lookup"><span data-stu-id="7b492-103">How to use Key Vault soft-delete with PowerShell</span></span>

<span data-ttu-id="7b492-104">Funktionen för mjuk borttagning av Azure Key Vault kan återställning av borttagna valv och valvet objekt.</span><span class="sxs-lookup"><span data-stu-id="7b492-104">Azure Key Vault's soft delete feature allows recovery of deleted vaults and vault objects.</span></span> <span data-ttu-id="7b492-105">Mer specifikt mjuk borttagning adresser i följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="7b492-105">Specifically, soft-delete addresses the following scenarios:</span></span>

- <span data-ttu-id="7b492-106">Stöd för återställningsbara borttagningen av ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="7b492-106">Support for recoverable deletion of a key vault</span></span>
- <span data-ttu-id="7b492-107">Stöd för återställningsbara borttagning av objekt i nyckelvalvet. nycklar hemligheter, och certifikat</span><span class="sxs-lookup"><span data-stu-id="7b492-107">Support for recoverable deletion of key vault objects; keys, secrets, and, certificates</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b492-108">Krav</span><span class="sxs-lookup"><span data-stu-id="7b492-108">Prerequisites</span></span>

- <span data-ttu-id="7b492-109">Azure PowerShell 4.0.0 eller senare - om du inte redan har gjort det här installationsprogrammet, installera Azure PowerShell och koppla den till din Azure-prenumeration, se [hur du installerar och konfigurerar du Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7b492-109">Azure PowerShell 4.0.0 or later - If you don't have this already setup, install Azure PowerShell and associate it with your Azure subscription, see [How to install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span> 

>[!NOTE]
> <span data-ttu-id="7b492-110">Det finns en inaktuell version av våra Key Vault PowerShell-utdata formatering fil som **kan** läsas in i din miljö i stället för rätt version.</span><span class="sxs-lookup"><span data-stu-id="7b492-110">There is an outdated version of our Key Vault PowerShell output formatting file that **may** be loaded into your environment instead of the correct version.</span></span> <span data-ttu-id="7b492-111">Vi förutse en uppdaterad version av PowerShell som innehåller nödvändiga korrigeringen för formatering av utdata och kommer att uppdatera det här avsnittet som för närvarande.</span><span class="sxs-lookup"><span data-stu-id="7b492-111">We are anticipating an updated version of PowerShell to contain the needed correction for the output formatting and will update this topic at that time.</span></span> <span data-ttu-id="7b492-112">Den aktuella lösningen bör du stöta på problemet formatering är:</span><span class="sxs-lookup"><span data-stu-id="7b492-112">The current workaround, should you encounter this formatting problem, is:</span></span>
> - <span data-ttu-id="7b492-113">Använd följande fråga om du upptäcker att det inte uppstår mjuk borttagning enabled-egenskap som beskrivs i det här avsnittet: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span><span class="sxs-lookup"><span data-stu-id="7b492-113">Use the following query if you notice you're not seeing the soft-delete enabled property described in this topic: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span></span>


<span data-ttu-id="7b492-114">Key Vault specifika refernece information för PowerShell finns i [Azure Key Vault PowerShell-referens](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span><span class="sxs-lookup"><span data-stu-id="7b492-114">For Key Vault specific refernece information for PowerShell, see [Azure Key Vault PowerShell reference](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span></span>

## <a name="required-permissions"></a><span data-ttu-id="7b492-115">Behörigheter som krävs</span><span class="sxs-lookup"><span data-stu-id="7b492-115">Required permissions</span></span>

<span data-ttu-id="7b492-116">Key Vault hanteras separat via rollbaserade behörigheter för åtkomstkontroll (RBAC) enligt följande:</span><span class="sxs-lookup"><span data-stu-id="7b492-116">Key Vault operations are separately managed via role-based access control (RBAC) permissions as follows:</span></span>

| <span data-ttu-id="7b492-117">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="7b492-117">Operation</span></span> | <span data-ttu-id="7b492-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7b492-118">Description</span></span> | <span data-ttu-id="7b492-119">Användarbehörighet</span><span class="sxs-lookup"><span data-stu-id="7b492-119">User permission</span></span> |
|:--|:--|:--|
|<span data-ttu-id="7b492-120">Visa lista</span><span class="sxs-lookup"><span data-stu-id="7b492-120">List</span></span>|<span data-ttu-id="7b492-121">Visar bort nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="7b492-121">Lists deleted key vaults.</span></span>|<span data-ttu-id="7b492-122">Microsoft.KeyVault/deletedVaults/read</span><span class="sxs-lookup"><span data-stu-id="7b492-122">Microsoft.KeyVault/deletedVaults/read</span></span>|
|<span data-ttu-id="7b492-123">Återställ</span><span class="sxs-lookup"><span data-stu-id="7b492-123">Recover</span></span>|<span data-ttu-id="7b492-124">Återställer borttagna nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7b492-124">Restores a deleted key vault.</span></span>|<span data-ttu-id="7b492-125">Microsoft.KeyVault/vaults/write</span><span class="sxs-lookup"><span data-stu-id="7b492-125">Microsoft.KeyVault/vaults/write</span></span>|
|<span data-ttu-id="7b492-126">Rensa</span><span class="sxs-lookup"><span data-stu-id="7b492-126">Purge</span></span>|<span data-ttu-id="7b492-127">Tar permanent bort borttagna nyckelvalvet och allt dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="7b492-127">Permanently removes a deleted key vault and all its contents.</span></span>|<span data-ttu-id="7b492-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span><span class="sxs-lookup"><span data-stu-id="7b492-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span></span>|

<span data-ttu-id="7b492-129">Mer information om behörighet och åtkomstkontroll finns [Secure nyckelvalvet](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="7b492-129">For more information on permissions and access control, see [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>

## <a name="enabling-soft-delete"></a><span data-ttu-id="7b492-130">Aktivera mjuk borttagning</span><span class="sxs-lookup"><span data-stu-id="7b492-130">Enabling soft-delete</span></span>

<span data-ttu-id="7b492-131">Om du vill kunna återställa borttagna nyckelvalv eller objekt som lagras i ett nyckelvalv måste du först aktivera mjuk borttagning för att nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7b492-131">To be able to recover a deleted key vault or objects stored in a key vault, you must first enable soft-delete for that key vault.</span></span>

### <a name="existing-key-vault"></a><span data-ttu-id="7b492-132">Befintliga nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="7b492-132">Existing key vault</span></span>

<span data-ttu-id="7b492-133">Aktivera mjuk borttagning på följande sätt för en befintlig nyckelvalv med namnet ContosoVault.</span><span class="sxs-lookup"><span data-stu-id="7b492-133">For an existing key vault named ContosoVault, enable soft-delete as follows.</span></span> 

>[!NOTE]
><span data-ttu-id="7b492-134">För närvarande måste du använda Azure Resource Manager resurs manipulering direkt skriva den *enableSoftDelete* egenskap till Key Vault-resurs.</span><span class="sxs-lookup"><span data-stu-id="7b492-134">Currently you need to use Azure Resource Manager resource manipulation to directly write the *enableSoftDelete* property to the Key Vault resource.</span></span>

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a><span data-ttu-id="7b492-135">Nytt nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="7b492-135">New key vault</span></span>

<span data-ttu-id="7b492-136">Aktivera mjuk borttagning för ett nytt nyckelvalv görs vid skapandet genom att lägga till soft-ta bort flaggan till din skapa-kommando.</span><span class="sxs-lookup"><span data-stu-id="7b492-136">Enabling soft-delete for a new key vault is done at creation time by adding the soft-delete enable flag to your create command.</span></span>

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a><span data-ttu-id="7b492-137">Kontrollera mjuk borttagning aktivering</span><span class="sxs-lookup"><span data-stu-id="7b492-137">Verify soft-delete enablement</span></span>

<span data-ttu-id="7b492-138">Om du vill kontrollera att ett nyckelvalv har aktiverat soft-delete, kör den *hämta* kommandot och leta efter den 'ej permanent ta bort Enabled?'</span><span class="sxs-lookup"><span data-stu-id="7b492-138">To verify that a key vault has soft-delete enabled, run the *get* command and look for the 'Soft Delete Enabled?'</span></span> <span data-ttu-id="7b492-139">attribut och dess inställning, true eller false.</span><span class="sxs-lookup"><span data-stu-id="7b492-139">attribute and its setting, true or false.</span></span>

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a><span data-ttu-id="7b492-140">Om du tar bort ett nyckelvalv som skyddas av mjuk borttagning</span><span class="sxs-lookup"><span data-stu-id="7b492-140">Deleting a key vault protected by soft-delete</span></span>

<span data-ttu-id="7b492-141">Kommandot för att ta bort (eller ta bort) en nyckelvalv förblir detsamma, men dess beteende ändras beroende på om du har aktiverat soft-ta bort eller inte.</span><span class="sxs-lookup"><span data-stu-id="7b492-141">The command to delete (or remove) a key vault remains the same, but its behavior changes depending on whether you have enabled soft-delete or not.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
><span data-ttu-id="7b492-142">Om du kör det föregående kommandot för ett nyckelvalv som inte har aktiverat soft-delete tar permanent bort den här nyckelvalvet och allt dess innehåll utan alternativ för återställning.</span><span class="sxs-lookup"><span data-stu-id="7b492-142">If you run the previous command for a key vault that does not have soft-delete enabled, you will permanently delete this key vault and all its content without any options for recovery.</span></span>

### <a name="how-soft-delete-protects-your-key-vaults"></a><span data-ttu-id="7b492-143">Hur mjuk borttagning skyddar ditt nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="7b492-143">How soft-delete protects your key vaults</span></span>

<span data-ttu-id="7b492-144">Aktiverad med mjuk borttagning:</span><span class="sxs-lookup"><span data-stu-id="7b492-144">With soft-delete enabled:</span></span>

- <span data-ttu-id="7b492-145">När en nyckelvalv tas bort, den tas bort från dess resursgrupp och placeras i ett reserverat namnområde som endast som är kopplade till den plats där den skapades.</span><span class="sxs-lookup"><span data-stu-id="7b492-145">When a key vault is deleted, it is removed from its resource group and placed in a reserved namespace that is only associated with the location where it was created.</span></span> 
- <span data-ttu-id="7b492-146">Objekt i en borttagen för valvet, t.ex. nycklar, hemligheter och certifikat, är tillgänglig och förblir det medan sina som innehåller nyckelvalv är i tillståndet deleted.</span><span class="sxs-lookup"><span data-stu-id="7b492-146">Objects in a deleted key vault, such as keys, secrets and, certificates, are inaccessible and remain so while their containing key vault is in the deleted state.</span></span> 
- <span data-ttu-id="7b492-147">DNS-namn för nyckelvalvet i ett borttaget tillstånd är kvar så det inte går att skapa ett nytt nyckelvalv med samma namn.</span><span class="sxs-lookup"><span data-stu-id="7b492-147">The DNS name for a key vault in a deleted state is still reserved so, a new key vault with same name cannot be created.</span></span>  

<span data-ttu-id="7b492-148">Du kan visa nyckelvalv i Borttaget tillstånd, som är associerade med din prenumeration, med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7b492-148">You may view deleted state key vaults, associated with your subscription, using the following command:</span></span>

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

<span data-ttu-id="7b492-149">Den *resurs-ID* i utdata som refererar till den ursprungliga resurs-ID för det här valvet.</span><span class="sxs-lookup"><span data-stu-id="7b492-149">The *Resource ID* in the output refers to the original resource ID of this vault.</span></span> <span data-ttu-id="7b492-150">Eftersom den här nyckelvalv är nu i ett borttaget tillstånd, det finns någon resurs med resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="7b492-150">Since this key vault is now in a deleted state, no resource exists with that resource ID.</span></span> <span data-ttu-id="7b492-151">Den *Id* fält kan användas för att identifiera resursen när du återställer eller rensa.</span><span class="sxs-lookup"><span data-stu-id="7b492-151">The *Id* field can be used to identify the resource when recovering, or purging.</span></span> <span data-ttu-id="7b492-152">Den *schemalagda Rensa datum* anger när valvet tas bort permanent (rensas) om ingen åtgärd utförs för borttagna valvet.</span><span class="sxs-lookup"><span data-stu-id="7b492-152">The *Scheduled Purge Date* field indicates when the vault will be permanently deleted (purged) if no action is taken for this deleted vault.</span></span> <span data-ttu-id="7b492-153">Loggperioden som används för att beräkna den *schemalagda Rensa datum*, är 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="7b492-153">The default retention period, used to calculate the *Scheduled Purge Date*, is 90 days.</span></span>

## <a name="recovering-a-key-vault"></a><span data-ttu-id="7b492-154">Återställa en nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="7b492-154">Recovering a key vault</span></span>

<span data-ttu-id="7b492-155">Om du vill återställa ett nyckelvalv som du behöver ange nyckelvalv namn, resursgruppens namn och plats.</span><span class="sxs-lookup"><span data-stu-id="7b492-155">To recover a key vault, you need to specify the key vault name, resource group, and location.</span></span> <span data-ttu-id="7b492-156">Notera platsen och resursgruppen för borttagna nyckelvalvet som du behöver dem för en nyckelvalv återställningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="7b492-156">Note the location and the resource group of the deleted key vault as you need these for a key vault recovery process.</span></span>

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

<span data-ttu-id="7b492-157">När en nyckelvalv återställs är resultatet en ny resurs med det nyckelvalv ursprungliga resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="7b492-157">When a key vault is recovered, the result is a new resource with the key vault's original resource ID.</span></span> <span data-ttu-id="7b492-158">Om resursgruppen där nyckelvalvet fanns har tagits bort, måste en ny resursgrupp med samma namn skapas innan nyckelvalvet kan återställas.</span><span class="sxs-lookup"><span data-stu-id="7b492-158">If the resource group where the key vault existed has been removed, a new resource group with same name must be created before the key vault can be recovered.</span></span>

## <a name="key-vault-objects-and-soft-delete"></a><span data-ttu-id="7b492-159">Key Vault-objekt och Mjuk borttagning</span><span class="sxs-lookup"><span data-stu-id="7b492-159">Key Vault objects and soft-delete</span></span>

<span data-ttu-id="7b492-160">För en nyckel ContosoFirstKey, i en källnyckelvalvets med namnet 'ContosoVault' med mjuk borttagning aktiverad här hur du skulle ta bort nyckeln.</span><span class="sxs-lookup"><span data-stu-id="7b492-160">For a key, 'ContosoFirstKey', in a key vault named 'ContosoVault' with soft-delete enabled, here's how you would delete that key.</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="7b492-161">Med nyckelvalvet aktiverad för mjuk borttagning, visas borttagna nyckeln fortfarande som om det har tagits bort utom, när du inte uttryckligen lista eller hämta borttagna nycklar.</span><span class="sxs-lookup"><span data-stu-id="7b492-161">With your key vault enabled for soft-delete, a deleted key still appears like it's deleted except, when you explicitly list or retrieve deleted keys.</span></span> <span data-ttu-id="7b492-162">De flesta åtgärder på en nyckel i tillståndet deleted misslyckas förutom visar en lista över borttagna nyckeln, återställs eller rensa den.</span><span class="sxs-lookup"><span data-stu-id="7b492-162">Most operations on a key in the deleted state will fail except for listing a deleted key, recovering it or purging it.</span></span> 

<span data-ttu-id="7b492-163">Till exempel om du vill begära att listan bort nycklar i en nyckelvalvet, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7b492-163">For example, to request to list deleted keys in a key vault, use the following command:</span></span>

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a><span data-ttu-id="7b492-164">Övergångstillstånd</span><span class="sxs-lookup"><span data-stu-id="7b492-164">Transition state</span></span> 

<span data-ttu-id="7b492-165">När du tar bort en nyckel i ett nyckelvalv med aktiverat soft bort kan det ta några sekunder för övergången.</span><span class="sxs-lookup"><span data-stu-id="7b492-165">When you delete a key in a key vault with soft-delete enabled, it may take a few seconds for the transition to complete.</span></span> <span data-ttu-id="7b492-166">Under den här övergångstillstånd kan visas att nyckeln inte är i aktivt läge eller Borttaget tillstånd.</span><span class="sxs-lookup"><span data-stu-id="7b492-166">During this transition state, it may appear that the key is not in the active state or the deleted state.</span></span> <span data-ttu-id="7b492-167">Det här kommandot visar en lista över alla borttagna nycklar i nyckelvalvet med namnet 'ContosoVault'.</span><span class="sxs-lookup"><span data-stu-id="7b492-167">This command will list all deleted keys in your key vault named 'ContosoVault'.</span></span>

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

### <a name="using-soft-delete-with-key-vault-objects"></a><span data-ttu-id="7b492-168">Hur mjuk borttagning med nyckelvalv objekt</span><span class="sxs-lookup"><span data-stu-id="7b492-168">Using soft-delete with key vault objects</span></span>

<span data-ttu-id="7b492-169">Precis som nyckelvalv, en borttagen nyckel hemlighet eller certifikat finns kvar i Borttaget tillstånd för upp till 90 dagar såvida du inte återställa den eller rensa den.</span><span class="sxs-lookup"><span data-stu-id="7b492-169">Just like key vaults, a deleted key, secret or, certificate will remain in deleted state for up to 90 days unless you recover it or purge it.</span></span> 

#### <a name="keys"></a><span data-ttu-id="7b492-170">Nycklar</span><span class="sxs-lookup"><span data-stu-id="7b492-170">Keys</span></span>

<span data-ttu-id="7b492-171">För att återställa en borttagen nyckel:</span><span class="sxs-lookup"><span data-stu-id="7b492-171">To recover a deleted key:</span></span>

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="7b492-172">Du ta bort en nyckel:</span><span class="sxs-lookup"><span data-stu-id="7b492-172">To permanently delete a key:</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
><span data-ttu-id="7b492-173">Rensa en nyckel tar permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="7b492-173">Purging a key will permanently delete it, meaning it will not be recoverable.</span></span>

<span data-ttu-id="7b492-174">Den **återställa** och **Rensa** åtgärder har sina egna behörigheter i en åtkomstprincip för nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="7b492-174">The **recover** and **purge** actions have their own permissions associated in a key vault access policy.</span></span> <span data-ttu-id="7b492-175">För en användare eller tjänstens huvudnamn för att kunna köra en **återställa** eller **Rensa** åtgärd som de måste ha behörigheten respektive för objektet (nyckel eller hemlighet) i åtkomstprincipen nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7b492-175">For a user or service principal to be able to execute a **recover** or **purge** action they must have the respective permission for that object (key or secret) in the key vault access policy.</span></span> <span data-ttu-id="7b492-176">Som standard den **Rensa** behörighet läggs inte till ett nyckelvalv åtkomstprincip genvägen till 'all' används för att ge alla behörigheter till en användare.</span><span class="sxs-lookup"><span data-stu-id="7b492-176">By default, the **purge** permission is not added to a key vault's access policy when the 'all' shortcut is used to grant all permissions to a user.</span></span> <span data-ttu-id="7b492-177">Du måste uttryckligen bevilja **Rensa** behörighet.</span><span class="sxs-lookup"><span data-stu-id="7b492-177">You must explicitly grant **purge** permission.</span></span> <span data-ttu-id="7b492-178">Till exempel följande kommando ger user@contoso.com behörighet att utföra flera åtgärder med nycklar i *ContosoVault* inklusive **Rensa**.</span><span class="sxs-lookup"><span data-stu-id="7b492-178">For example, the following command grants user@contoso.com permission to perform several operations on keys in *ContosoVault* including **purge**.</span></span>

#### <a name="set-a-key-vault-access-policy"></a><span data-ttu-id="7b492-179">Skapa en princip för nyckelvalvet åtkomst</span><span class="sxs-lookup"><span data-stu-id="7b492-179">Set a key vault access policy</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> <span data-ttu-id="7b492-180">Om du har en befintlig nyckelvalv som precis har haft soft-ta bort aktiverat kan du kanske inte har **återställa** och **Rensa** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="7b492-180">If you have an existing key vault that has just had soft-delete enabled, you may not have **recover** and **purge** permissions.</span></span>

#### <a name="secrets"></a><span data-ttu-id="7b492-181">Hemligheter</span><span class="sxs-lookup"><span data-stu-id="7b492-181">Secrets</span></span>

<span data-ttu-id="7b492-182">Som nycklar drivs hemligheter i en nyckelvalvet på med sina egna kommandon.</span><span class="sxs-lookup"><span data-stu-id="7b492-182">Like keys, secrets in a key vault are operated on with their own commands.</span></span> <span data-ttu-id="7b492-183">Följande, är kommandon för att ta bort, lista, återställa och rensa hemligheter.</span><span class="sxs-lookup"><span data-stu-id="7b492-183">Following, are the commands for deleting, listing, recovering, and purging secrets.</span></span>

- <span data-ttu-id="7b492-184">Ta bort en hemlighet som heter SQLPassword:</span><span class="sxs-lookup"><span data-stu-id="7b492-184">Delete a secret named SQLPassword:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- <span data-ttu-id="7b492-185">Lista alla borttagna hemligheter i en nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="7b492-185">List all deleted secrets in a key vault:</span></span> 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- <span data-ttu-id="7b492-186">Återställa en hemlighet i Borttaget tillstånd:</span><span class="sxs-lookup"><span data-stu-id="7b492-186">Recover a secret in the deleted state:</span></span> 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- <span data-ttu-id="7b492-187">Rensa en hemlighet i Borttaget tillstånd:</span><span class="sxs-lookup"><span data-stu-id="7b492-187">Purge a secret in deleted state:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
><span data-ttu-id="7b492-188">Rensa en hemlighet tar permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="7b492-188">Purging a secret will permanently delete it, meaning it will not be recoverable.</span></span>

## <a name="purging-and-key-vaults"></a><span data-ttu-id="7b492-189">Ovanstående och nyckel valv</span><span class="sxs-lookup"><span data-stu-id="7b492-189">Purging and key vaults</span></span>

### <a name="key-vault-objects"></a><span data-ttu-id="7b492-190">Nyckelvalv objekt</span><span class="sxs-lookup"><span data-stu-id="7b492-190">Key vault objects</span></span>

<span data-ttu-id="7b492-191">Rensa en nyckel tar hemlighet eller certifikat permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="7b492-191">Purging a key, secret or, certificate will permanently delete it, meaning it will not be recoverable.</span></span> <span data-ttu-id="7b492-192">Nyckelvalvet med borttagna objekt förblir dock intakta som kommer alla andra objekt i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7b492-192">The key vault that contained the deleted object will however remain intact as will all other objects in the key vault.</span></span> 

### <a name="key-vaults-as-containers"></a><span data-ttu-id="7b492-193">Nyckel-valv som behållare</span><span class="sxs-lookup"><span data-stu-id="7b492-193">Key vaults as containers</span></span>
<span data-ttu-id="7b492-194">När en nyckelvalv rensas tas allt innehåll, inklusive nycklar och hemligheter certifikat bort permanent.</span><span class="sxs-lookup"><span data-stu-id="7b492-194">When a key vault is purged, all of its contents, including keys, secrets, and certificates, are permanently deleted.</span></span> <span data-ttu-id="7b492-195">Om du vill rensa en key vault använder den `Remove-AzureRmKeyVault` kommandot med alternativet `-InRemovedState` och genom att ange platsen för borttagna nyckelvalvet med den `-Location location` argumentet.</span><span class="sxs-lookup"><span data-stu-id="7b492-195">To purge a key vault, use the `Remove-AzureRmKeyVault` command with the option `-InRemovedState` and by specifying the location of the deleted key vault with the `-Location location` argument.</span></span> <span data-ttu-id="7b492-196">Du kan hitta platsen för en borttagen valvet med hjälp av kommandot `Get-AzureRmKeyVault -InRemovedState`.</span><span class="sxs-lookup"><span data-stu-id="7b492-196">You can find the location of a deleted vault using the command `Get-AzureRmKeyVault -InRemovedState`.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
><span data-ttu-id="7b492-197">Rensa en key vault tar permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="7b492-197">Purging a key vault will permanently delete it, meaning it will not be recoverable.</span></span>

### <a name="purge-permissions-required"></a><span data-ttu-id="7b492-198">Rensa behörigheter som krävs</span><span class="sxs-lookup"><span data-stu-id="7b492-198">Purge permissions required</span></span>
- <span data-ttu-id="7b492-199">Om du vill rensa borttagna nyckelvalvet, så att valvet och allt dess innehåll tas bort permanent, måste användaren RBAC-behörighet för att utföra en *Microsoft.KeyVault/locations/deletedVaults/purge/action* igen.</span><span class="sxs-lookup"><span data-stu-id="7b492-199">To purge a deleted key vault, such that the vault and all its contents are permanently removed, the user needs RBAC permission to perform a *Microsoft.KeyVault/locations/deletedVaults/purge/action* operation.</span></span> 
- <span data-ttu-id="7b492-200">Om du vill visa den borttagna nyckeln valvet en användare behöver RBAC-behörighet för att utföra *Microsoft.KeyVault/deletedVaults/read* behörighet.</span><span class="sxs-lookup"><span data-stu-id="7b492-200">To list the deleted key, the vault a user needs RBAC permission to perform *Microsoft.KeyVault/deletedVaults/read* permission.</span></span> 
- <span data-ttu-id="7b492-201">Som standard har bara en prenumeration administratör dessa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="7b492-201">By default only a subscription administrator has these permissions.</span></span> 

### <a name="scheduled-purge"></a><span data-ttu-id="7b492-202">Schemalagda Rensa</span><span class="sxs-lookup"><span data-stu-id="7b492-202">Scheduled purge</span></span>

<span data-ttu-id="7b492-203">Visar en lista över borttagna nyckelvalv-objekt visar när de är schedled tas bort av Key Vault.</span><span class="sxs-lookup"><span data-stu-id="7b492-203">Listing your deleted key vault objects shows when they are schedled to be purged by Key Vault.</span></span> <span data-ttu-id="7b492-204">Den *schemalagda Rensa datum* fältet visas när ett nyckelvalv objekt tas bort permanent, om ingen åtgärd utförs.</span><span class="sxs-lookup"><span data-stu-id="7b492-204">The *Scheduled Purge Date* field indicates when a key vault object will be permanently deleted, if no action is taken.</span></span> <span data-ttu-id="7b492-205">Som standard är loggperioden för ett objekt för borttagna nyckelvalv 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="7b492-205">By default, the retention period for a deleted key vault object is 90 days.</span></span>

>[!NOTE]
><span data-ttu-id="7b492-206">Ett objekt för borttagna valvet utlöses av dess *schemalagda Rensa datum* fältet, bort permanent.</span><span class="sxs-lookup"><span data-stu-id="7b492-206">A purged vault object, triggered by its *Scheduled Purge Date* field, is permanently deleted.</span></span> <span data-ttu-id="7b492-207">Den kan inte återställas.</span><span class="sxs-lookup"><span data-stu-id="7b492-207">It is not recoverable.</span></span>

## <a name="other-resources"></a><span data-ttu-id="7b492-208">Andra resurser</span><span class="sxs-lookup"><span data-stu-id="7b492-208">Other resources</span></span>

- <span data-ttu-id="7b492-209">En översikt över funktionen för mjuk borttagning av Key Vault finns [översikt över Azure Key Vault-soft-ta bort](key-vault-ovw-soft-delete.md).</span><span class="sxs-lookup"><span data-stu-id="7b492-209">For an overview of Key Vault's soft-delete feature, see [Azure Key Vault soft-delete overview](key-vault-ovw-soft-delete.md).</span></span>
- <span data-ttu-id="7b492-210">En allmän översikt över användning av Azure Key Vault finns [Kom igång med Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7b492-210">For a general overview of Azure Key Vault usage, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>

