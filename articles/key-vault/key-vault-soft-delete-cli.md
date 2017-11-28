---
ms.assetid: 
title: "aaaAzure nyckel valvet – hur toouse mjuka bort med CLI"
description: "Använda case exempel på mjuk borttagning med CLI-kodstyckena"
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: bruceper
ms.openlocfilehash: 672f5210ab119c244ca712f0bb80b653b50ea79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-key-vault-soft-delete-with-cli"></a><span data-ttu-id="19a18-103">Hur toouse Key Vault mjuk borttagning med CLI</span><span class="sxs-lookup"><span data-stu-id="19a18-103">How toouse Key Vault soft-delete with CLI</span></span>

<span data-ttu-id="19a18-104">Funktionen för mjuk borttagning av Azure Key Vault kan återställning av borttagna valv och valvet objekt.</span><span class="sxs-lookup"><span data-stu-id="19a18-104">Azure Key Vault's soft delete feature allows recovery of deleted vaults and vault objects.</span></span> <span data-ttu-id="19a18-105">Mer specifikt mjuk borttagning adresser hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="19a18-105">Specifically, soft-delete addresses hello following scenarios:</span></span>

- <span data-ttu-id="19a18-106">Stöd för återställningsbara borttagningen av ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="19a18-106">Support for recoverable deletion of a key vault</span></span>
- <span data-ttu-id="19a18-107">Stöd för återställningsbara borttagning av objekt i nyckelvalvet. nycklar hemligheter, och certifikat</span><span class="sxs-lookup"><span data-stu-id="19a18-107">Support for recoverable deletion of key vault objects; keys, secrets, and, certificates</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19a18-108">Krav</span><span class="sxs-lookup"><span data-stu-id="19a18-108">Prerequisites</span></span>

- <span data-ttu-id="19a18-109">Azure CLI 2.0 - om du inte har den här installationen för din miljö finns [hantera Nyckelvalv med hjälp av CLI 2.0](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="19a18-109">Azure CLI 2.0 - If you don't have this setup for your environment, see [Manage Key Vault using CLI 2.0](key-vault-manage-with-cli2.md).</span></span>

<span data-ttu-id="19a18-110">Key Vault specifika referensinformation för CLI, se [Azure CLI 2.0 Key Vault referens](https://docs.microsoft.com/cli/azure/keyvault).</span><span class="sxs-lookup"><span data-stu-id="19a18-110">For Key Vault specific reference information for CLI, see [Azure CLI 2.0 Key Vault reference](https://docs.microsoft.com/cli/azure/keyvault).</span></span>

## <a name="required-permissions"></a><span data-ttu-id="19a18-111">Behörigheter som krävs</span><span class="sxs-lookup"><span data-stu-id="19a18-111">Required permissions</span></span>

<span data-ttu-id="19a18-112">Key Vault hanteras separat via rollbaserade behörigheter för åtkomstkontroll (RBAC) enligt följande:</span><span class="sxs-lookup"><span data-stu-id="19a18-112">Key Vault operations are separately managed via role-based access control (RBAC) permissions as follows:</span></span>

| <span data-ttu-id="19a18-113">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="19a18-113">Operation</span></span> | <span data-ttu-id="19a18-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="19a18-114">Description</span></span> | <span data-ttu-id="19a18-115">Användarbehörighet</span><span class="sxs-lookup"><span data-stu-id="19a18-115">User permission</span></span> |
|:--|:--|:--|
|<span data-ttu-id="19a18-116">Visa lista</span><span class="sxs-lookup"><span data-stu-id="19a18-116">List</span></span>|<span data-ttu-id="19a18-117">Visar bort nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="19a18-117">Lists deleted key vaults.</span></span>|<span data-ttu-id="19a18-118">Microsoft.KeyVault/deletedVaults/read</span><span class="sxs-lookup"><span data-stu-id="19a18-118">Microsoft.KeyVault/deletedVaults/read</span></span>|
|<span data-ttu-id="19a18-119">Återställ</span><span class="sxs-lookup"><span data-stu-id="19a18-119">Recover</span></span>|<span data-ttu-id="19a18-120">Återställer borttagna nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="19a18-120">Restores a deleted key vault.</span></span>|<span data-ttu-id="19a18-121">Microsoft.KeyVault/vaults/write</span><span class="sxs-lookup"><span data-stu-id="19a18-121">Microsoft.KeyVault/vaults/write</span></span>|
|<span data-ttu-id="19a18-122">Rensa</span><span class="sxs-lookup"><span data-stu-id="19a18-122">Purge</span></span>|<span data-ttu-id="19a18-123">Tar permanent bort borttagna nyckelvalvet och allt dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="19a18-123">Permanently removes a deleted key vault and all its contents.</span></span>|<span data-ttu-id="19a18-124">Microsoft.KeyVault/locations/deletedVaults/purge/action</span><span class="sxs-lookup"><span data-stu-id="19a18-124">Microsoft.KeyVault/locations/deletedVaults/purge/action</span></span>|

<span data-ttu-id="19a18-125">Mer information om behörighet och åtkomstkontroll finns [Secure nyckelvalvet](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="19a18-125">For more information on permissions and access control, see [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>

## <a name="enabling-soft-delete"></a><span data-ttu-id="19a18-126">Aktivera mjuk borttagning</span><span class="sxs-lookup"><span data-stu-id="19a18-126">Enabling soft-delete</span></span>

<span data-ttu-id="19a18-127">toobe kan toorecover borttagna nyckelvalv eller objekt som lagras i en nyckel valvet, måste du först aktivera mjuk borttagning för att nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="19a18-127">toobe able toorecover a deleted key vault or objects stored in a key vault, you must first enable soft-delete for that key vault.</span></span>

### <a name="existing-key-vault"></a><span data-ttu-id="19a18-128">Befintliga nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="19a18-128">Existing key vault</span></span>

<span data-ttu-id="19a18-129">Aktivera mjuk borttagning på följande sätt för en befintlig nyckelvalv med namnet ContosoVault.</span><span class="sxs-lookup"><span data-stu-id="19a18-129">For an existing key vault named ContosoVault, enable soft-delete as follows.</span></span> 

>[!NOTE]
><span data-ttu-id="19a18-130">För närvarande måste toouse Azure Resource Manager resurs manipulering toodirectly skrivåtgärder hello *enableSoftDelete* egenskapen toohello Key Vault-resurs.</span><span class="sxs-lookup"><span data-stu-id="19a18-130">Currently you need toouse Azure Resource Manager resource manipulation toodirectly write hello *enableSoftDelete* property toohello Key Vault resource.</span></span>

```azurecli
az resource update --id $(az keyvault show --name ContosoVault -o tsv | awk '{print $1}') --set properties.enableSoftDelete=true
```

### <a name="new-key-vault"></a><span data-ttu-id="19a18-131">Nytt nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="19a18-131">New key vault</span></span>

<span data-ttu-id="19a18-132">Aktivera mjuk borttagning för ett nytt nyckelvalv görs vid skapandet genom att lägga till hello soft-ta bort flaggan tooyour skapa kommando.</span><span class="sxs-lookup"><span data-stu-id="19a18-132">Enabling soft-delete for a new key vault is done at creation time by adding hello soft-delete enable flag tooyour create command.</span></span>

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --enable-soft-delete true --location westus
```

### <a name="verify-soft-delete-enablement"></a><span data-ttu-id="19a18-133">Kontrollera mjuk borttagning aktivering</span><span class="sxs-lookup"><span data-stu-id="19a18-133">Verify soft-delete enablement</span></span>

<span data-ttu-id="19a18-134">tooverify som ett nyckelvalv har aktiverat mjuk borttagning, kör hello *visa* kommandot och leta efter hello ”ej permanent ta bort aktiverat'</span><span class="sxs-lookup"><span data-stu-id="19a18-134">tooverify that a key vault has soft-delete enabled, run hello *show* command and look for hello 'Soft Delete Enabled?'</span></span> <span data-ttu-id="19a18-135">attribut och dess inställning, true eller false.</span><span class="sxs-lookup"><span data-stu-id="19a18-135">attribute and its setting, true or false.</span></span>

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a><span data-ttu-id="19a18-136">Om du tar bort ett nyckelvalv som skyddas av mjuk borttagning</span><span class="sxs-lookup"><span data-stu-id="19a18-136">Deleting a key vault protected by soft-delete</span></span>

<span data-ttu-id="19a18-137">hello kommandot toodelete (eller ta bort) en nyckelvalv förblir hello detsamma, men dess funktionsändringar beroende på om du har aktiverat soft-ta bort eller inte.</span><span class="sxs-lookup"><span data-stu-id="19a18-137">hello command toodelete (or remove) a key vault remains hello same, but its behavior changes depending on whether you have enabled soft-delete or not.</span></span>

```azurecli
az keyvault delete --name ContosoVault
```

> [!IMPORTANT]
><span data-ttu-id="19a18-138">Om du kör hello föregående kommando för ett nyckelvalv som inte har aktiverat soft-delete tar permanent bort den här nyckelvalvet och allt dess innehåll utan alternativ för återställning.</span><span class="sxs-lookup"><span data-stu-id="19a18-138">If you run hello previous command for a key vault that does not have soft-delete enabled, you will permanently delete this key vault and all its content without any options for recovery.</span></span>

### <a name="how-soft-delete-protects-your-key-vaults"></a><span data-ttu-id="19a18-139">Hur mjuk borttagning skyddar ditt nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="19a18-139">How soft-delete protects your key vaults</span></span>

<span data-ttu-id="19a18-140">Aktiverad med mjuk borttagning:</span><span class="sxs-lookup"><span data-stu-id="19a18-140">With soft-delete enabled:</span></span>

- <span data-ttu-id="19a18-141">När en nyckelvalv tas bort, den tas bort från dess resursgrupp och placeras i ett reserverat namnområde som endast är associerad med hello plats där den skapades.</span><span class="sxs-lookup"><span data-stu-id="19a18-141">When a key vault is deleted, it is removed from its resource group and placed in a reserved namespace that is only associated with hello location where it was created.</span></span> 
- <span data-ttu-id="19a18-142">Objekt i en borttagen för valvet, t.ex. nycklar, hemligheter och certifikat, är tillgänglig och förblir det medan sina som innehåller nyckelvalv hello bort tillstånd.</span><span class="sxs-lookup"><span data-stu-id="19a18-142">Objects in a deleted key vault, such as keys, secrets and, certificates, are inaccessible and remain so while their containing key vault is in hello deleted state.</span></span> 
- <span data-ttu-id="19a18-143">hello DNS-namn för nyckelvalvet i ett borttaget tillstånd är kvar så det inte går att skapa ett nytt nyckelvalv med samma namn.</span><span class="sxs-lookup"><span data-stu-id="19a18-143">hello DNS name for a key vault in a deleted state is still reserved so, a new key vault with same name cannot be created.</span></span>  

<span data-ttu-id="19a18-144">Du kan visa Borttaget tillstånd nyckelvalv, som är associerade med din prenumeration med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="19a18-144">You may view deleted state key vaults, associated with your subscription, using hello following command:</span></span>

```azurecli
az keyvault list-deleted
```

<span data-ttu-id="19a18-145">Hej *resurs-ID* i hello utdata refererar toohello ursprungliga resurs-ID för det här valvet.</span><span class="sxs-lookup"><span data-stu-id="19a18-145">hello *Resource ID* in hello output refers toohello original resource ID of this vault.</span></span> <span data-ttu-id="19a18-146">Eftersom den här nyckelvalv är nu i ett borttaget tillstånd, det finns någon resurs med resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="19a18-146">Since this key vault is now in a deleted state, no resource exists with that resource ID.</span></span> <span data-ttu-id="19a18-147">Hej *Id* fältet kan vara används tooidentify hello resurs när du återställer eller rensa.</span><span class="sxs-lookup"><span data-stu-id="19a18-147">hello *Id* field can be used tooidentify hello resource when recovering, or purging.</span></span> <span data-ttu-id="19a18-148">Hej *schemalagda Rensa datum* anger när hello valvet tas bort permanent (rensas) om ingen åtgärd utförs för borttagna valvet.</span><span class="sxs-lookup"><span data-stu-id="19a18-148">hello *Scheduled Purge Date* field indicates when hello vault will be permanently deleted (purged) if no action is taken for this deleted vault.</span></span> <span data-ttu-id="19a18-149">hello standard kvarhållning period, använt toocalculate hello *schemalagda Rensa datum*, är 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="19a18-149">hello default retention period, used toocalculate hello *Scheduled Purge Date*, is 90 days.</span></span>

## <a name="recovering-a-key-vault"></a><span data-ttu-id="19a18-150">Återställa en nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="19a18-150">Recovering a key vault</span></span>

<span data-ttu-id="19a18-151">toorecover ett nyckelvalv måste toospecify hello nyckelvalv namn, resursgruppens namn och plats.</span><span class="sxs-lookup"><span data-stu-id="19a18-151">toorecover a key vault, you need toospecify hello key vault name, resource group, and location.</span></span> <span data-ttu-id="19a18-152">Obs hello plats och hello resursgruppen av hello bort nyckelvalv som du behöver dem för en nyckelvalv återställningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="19a18-152">Note hello location and hello resource group of hello deleted key vault as you need these for a key vault recovery process.</span></span>

```azurecli
az keyvault recover --location westus --name ContosoVault
```

<span data-ttu-id="19a18-153">När en nyckelvalv återställs är hello resultatet en ny resurs med hello nyckelvalv ursprungliga resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="19a18-153">When a key vault is recovered, hello result is a new resource with hello key vault's original resource ID.</span></span> <span data-ttu-id="19a18-154">Om hello resursgruppen där hello nyckelvalv fanns har tagits bort, måste en ny resursgrupp med samma namn skapas innan hello nyckelvalv kan återställas.</span><span class="sxs-lookup"><span data-stu-id="19a18-154">If hello resource group where hello key vault existed has been removed, a new resource group with same name must be created before hello key vault can be recovered.</span></span>

## <a name="key-vault-objects-and-soft-delete"></a><span data-ttu-id="19a18-155">Key Vault-objekt och Mjuk borttagning</span><span class="sxs-lookup"><span data-stu-id="19a18-155">Key Vault objects and soft-delete</span></span>

<span data-ttu-id="19a18-156">För en nyckel ContosoFirstKey, i en källnyckelvalvets med namnet 'ContosoVault' med mjuk borttagning aktiverad här hur du skulle ta bort nyckeln.</span><span class="sxs-lookup"><span data-stu-id="19a18-156">For a key, 'ContosoFirstKey', in a key vault named 'ContosoVault' with soft-delete enabled, here's how you would delete that key.</span></span>

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

<span data-ttu-id="19a18-157">Med nyckelvalvet aktiverad för mjuk borttagning, visas borttagna nyckeln fortfarande som om det har tagits bort utom, när du inte uttryckligen lista eller hämta borttagna nycklar.</span><span class="sxs-lookup"><span data-stu-id="19a18-157">With your key vault enabled for soft-delete, a deleted key still appears like it's deleted except, when you explicitly list or retrieve deleted keys.</span></span> <span data-ttu-id="19a18-158">De flesta åtgärder på en nyckel i hello bort tillstånd misslyckas förutom visar en lista över borttagna nyckeln, återställs eller rensa den.</span><span class="sxs-lookup"><span data-stu-id="19a18-158">Most operations on a key in hello deleted state will fail except for listing a deleted key, recovering it or purging it.</span></span> 

<span data-ttu-id="19a18-159">Till exempel använda toorequest toolist bort nycklar i nyckelvalvet, hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="19a18-159">For example, toorequest toolist deleted keys in a key vault, use hello following command:</span></span>

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a><span data-ttu-id="19a18-160">Övergångstillstånd</span><span class="sxs-lookup"><span data-stu-id="19a18-160">Transition state</span></span> 

<span data-ttu-id="19a18-161">Det kan ta några sekunder för hello övergången toocomplete när du tar bort en nyckel i ett nyckelvalv med aktiverad mjuk borttagning.</span><span class="sxs-lookup"><span data-stu-id="19a18-161">When you delete a key in a key vault with soft-delete enabled, it may take a few seconds for hello transition toocomplete.</span></span> <span data-ttu-id="19a18-162">Under den här övergångstillstånd kan visas hello nyckeln inte är i hello active eller hello bort tillstånd.</span><span class="sxs-lookup"><span data-stu-id="19a18-162">During this transition state, it may appear that hello key is not in hello active state or hello deleted state.</span></span> <span data-ttu-id="19a18-163">Det här kommandot visar en lista över alla borttagna nycklar i nyckelvalvet med namnet 'ContosoVault'.</span><span class="sxs-lookup"><span data-stu-id="19a18-163">This command will list all deleted keys in your key vault named 'ContosoVault'.</span></span>

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="using-soft-delete-with-key-vault-objects"></a><span data-ttu-id="19a18-164">Hur mjuk borttagning med nyckelvalv objekt</span><span class="sxs-lookup"><span data-stu-id="19a18-164">Using soft-delete with key vault objects</span></span>

<span data-ttu-id="19a18-165">Precis som nyckelvalv, en borttagen nyckel hemlighet eller certifikat finns kvar i Borttaget tillstånd för in too90 dagar såvida du inte återställa den eller rensa den.</span><span class="sxs-lookup"><span data-stu-id="19a18-165">Just like key vaults, a deleted key, secret or, certificate will remain in deleted state for up too90 days unless you recover it or purge it.</span></span> 

#### <a name="keys"></a><span data-ttu-id="19a18-166">Nycklar</span><span class="sxs-lookup"><span data-stu-id="19a18-166">Keys</span></span>

<span data-ttu-id="19a18-167">toorecover borttagna nyckeln:</span><span class="sxs-lookup"><span data-stu-id="19a18-167">toorecover a deleted key:</span></span>

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

<span data-ttu-id="19a18-168">toopermanently ta bort en nyckel:</span><span class="sxs-lookup"><span data-stu-id="19a18-168">toopermanently delete a key:</span></span>

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

>[!NOTE]
><span data-ttu-id="19a18-169">Rensa en nyckel tar permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="19a18-169">Purging a key will permanently delete it, meaning it will not be recoverable.</span></span>

<span data-ttu-id="19a18-170">Hej **återställa** och **Rensa** åtgärder har sina egna behörigheter i en åtkomstprincip för nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="19a18-170">hello **recover** and **purge** actions have their own permissions associated in a key vault access policy.</span></span> <span data-ttu-id="19a18-171">För en användare eller tjänst huvudnamn toobe kan tooexecute en **återställa** eller **Rensa** åtgärd som de måste ha hello respektive behörighet för objektet (nyckel eller hemlighet) i hello nyckelvalv åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="19a18-171">For a user or service principal toobe able tooexecute a **recover** or **purge** action they must have hello respective permission for that object (key or secret) in hello key vault access policy.</span></span> <span data-ttu-id="19a18-172">Som standard hello **Rensa** behörighet läggs inte tooa nyckelvalv åtkomstprincip hello 'all' genväg kan du använda toogrant alla behörigheter tooa användare.</span><span class="sxs-lookup"><span data-stu-id="19a18-172">By default, hello **purge** permission is not added tooa key vault's access policy when hello 'all' shortcut is used toogrant all permissions tooa user.</span></span> <span data-ttu-id="19a18-173">Du måste uttryckligen bevilja **Rensa** behörighet.</span><span class="sxs-lookup"><span data-stu-id="19a18-173">You must explicitly grant **purge** permission.</span></span> <span data-ttu-id="19a18-174">Till exempel hello efter kommandot ger user@contoso.com behörighet tooperform flera åtgärder med nycklar i *ContosoVault* inklusive **Rensa**.</span><span class="sxs-lookup"><span data-stu-id="19a18-174">For example, hello following command grants user@contoso.com permission tooperform several operations on keys in *ContosoVault* including **purge**.</span></span>

#### <a name="set-a-key-vault-access-policy"></a><span data-ttu-id="19a18-175">Skapa en princip för nyckelvalvet åtkomst</span><span class="sxs-lookup"><span data-stu-id="19a18-175">Set a key vault access policy</span></span>

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> <span data-ttu-id="19a18-176">Om du har en befintlig nyckelvalv som precis har haft soft-ta bort aktiverat kan du kanske inte har **återställa** och **Rensa** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="19a18-176">If you have an existing key vault that has just had soft-delete enabled, you may not have **recover** and **purge** permissions.</span></span>

#### <a name="secrets"></a><span data-ttu-id="19a18-177">Hemligheter</span><span class="sxs-lookup"><span data-stu-id="19a18-177">Secrets</span></span>

<span data-ttu-id="19a18-178">Som nycklar drivs hemligheter i en nyckelvalvet på med sina egna kommandon.</span><span class="sxs-lookup"><span data-stu-id="19a18-178">Like keys, secrets in a key vault are operated on with their own commands.</span></span> <span data-ttu-id="19a18-179">Följande, är hello-kommandon för att ta bort, lista, återställa och rensa hemligheter.</span><span class="sxs-lookup"><span data-stu-id="19a18-179">Following, are hello commands for deleting, listing, recovering, and purging secrets.</span></span>

- <span data-ttu-id="19a18-180">Ta bort en hemlighet som heter SQLPassword:</span><span class="sxs-lookup"><span data-stu-id="19a18-180">Delete a secret named SQLPassword:</span></span> 
```azurecli
az keyvault secret delete --vault-name ContosoVault -name SQLPassword
```

- <span data-ttu-id="19a18-181">Lista alla borttagna hemligheter i en nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="19a18-181">List all deleted secrets in a key vault:</span></span> 
```azurecli
az keyvault secret list-deleted --vault-name ContosoVault
```

- <span data-ttu-id="19a18-182">Återställa en hemlighet i hello bort tillstånd:</span><span class="sxs-lookup"><span data-stu-id="19a18-182">Recover a secret in hello deleted state:</span></span> 
```azurecli
az keyvault secret recover --name SQLPassword --vault-name ContosoVault
```

- <span data-ttu-id="19a18-183">Rensa en hemlighet i Borttaget tillstånd:</span><span class="sxs-lookup"><span data-stu-id="19a18-183">Purge a secret in deleted state:</span></span> 
```azurecli
az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
```

>[!NOTE]
><span data-ttu-id="19a18-184">Rensa en hemlighet tar permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="19a18-184">Purging a secret will permanently delete it, meaning it will not be recoverable.</span></span>

## <a name="purging-and-key-vaults"></a><span data-ttu-id="19a18-185">Ovanstående och nyckel valv</span><span class="sxs-lookup"><span data-stu-id="19a18-185">Purging and key vaults</span></span>

### <a name="key-vault-objects"></a><span data-ttu-id="19a18-186">Nyckelvalv objekt</span><span class="sxs-lookup"><span data-stu-id="19a18-186">Key vault objects</span></span>

<span data-ttu-id="19a18-187">Rensa en nyckel tar hemlighet eller certifikat permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="19a18-187">Purging a key, secret or, certificate will permanently delete it, meaning it will not be recoverable.</span></span> <span data-ttu-id="19a18-188">Hej nyckelvalv med hello borttagna objekt förblir men intakta som kommer alla andra objekt i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="19a18-188">hello key vault that contained hello deleted object will however remain intact as will all other objects in hello key vault.</span></span> 

### <a name="key-vaults-as-containers"></a><span data-ttu-id="19a18-189">Nyckel-valv som behållare</span><span class="sxs-lookup"><span data-stu-id="19a18-189">Key vaults as containers</span></span>
<span data-ttu-id="19a18-190">När en nyckelvalv rensas tas allt innehåll, inklusive nycklar och hemligheter certifikat bort permanent.</span><span class="sxs-lookup"><span data-stu-id="19a18-190">When a key vault is purged, all of its contents, including keys, secrets, and certificates, are permanently deleted.</span></span> <span data-ttu-id="19a18-191">toopurge nyckelvalvet, använda hello `az keyvault purge` kommando.</span><span class="sxs-lookup"><span data-stu-id="19a18-191">toopurge a key vault, use hello `az keyvault purge` command.</span></span> <span data-ttu-id="19a18-192">Du kan hitta hello plats din prenumeration borttagna nyckelvalv hello kommandot `az keyvault list-deleted`.</span><span class="sxs-lookup"><span data-stu-id="19a18-192">You can find hello location your subscription's deleted key vaults using hello command `az keyvault list-deleted`.</span></span>

```azurecli
az keyvault purge --location westus --name ContosoVault
```

>[!NOTE]
><span data-ttu-id="19a18-193">Rensa en key vault tar permanent bort, vilket innebär att det inte går att återställa.</span><span class="sxs-lookup"><span data-stu-id="19a18-193">Purging a key vault will permanently delete it, meaning it will not be recoverable.</span></span>

### <a name="purge-permissions-required"></a><span data-ttu-id="19a18-194">Rensa behörigheter som krävs</span><span class="sxs-lookup"><span data-stu-id="19a18-194">Purge permissions required</span></span>
- <span data-ttu-id="19a18-195">toopurge borttagna nyckelvalvet, så att hello valvet och allt dess innehåll tas bort permanent, hello användare behöver RBAC behörighet tooperform en *Microsoft.KeyVault/locations/deletedVaults/purge/action* igen.</span><span class="sxs-lookup"><span data-stu-id="19a18-195">toopurge a deleted key vault, such that hello vault and all its contents are permanently removed, hello user needs RBAC permission tooperform a *Microsoft.KeyVault/locations/deletedVaults/purge/action* operation.</span></span> 
- <span data-ttu-id="19a18-196">toolist hello bort nyckel hello valvet en användare behöver RBAC behörighet tooperform *Microsoft.KeyVault/deletedVaults/read* behörighet.</span><span class="sxs-lookup"><span data-stu-id="19a18-196">toolist hello deleted key, hello vault a user needs RBAC permission tooperform *Microsoft.KeyVault/deletedVaults/read* permission.</span></span> 
- <span data-ttu-id="19a18-197">Som standard har bara en prenumeration administratör dessa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="19a18-197">By default only a subscription administrator has these permissions.</span></span> 

### <a name="scheduled-purge"></a><span data-ttu-id="19a18-198">Schemalagda Rensa</span><span class="sxs-lookup"><span data-stu-id="19a18-198">Scheduled purge</span></span>

<span data-ttu-id="19a18-199">Visar en lista över borttagna nyckelvalv-objekt visar när de är schedled toobe rensas av Key Vault.</span><span class="sxs-lookup"><span data-stu-id="19a18-199">Listing your deleted key vault objects shows when they are schedled toobe purged by Key Vault.</span></span> <span data-ttu-id="19a18-200">Hej *schemalagda Rensa datum* fältet visas när ett nyckelvalv objekt tas bort permanent, om ingen åtgärd utförs.</span><span class="sxs-lookup"><span data-stu-id="19a18-200">hello *Scheduled Purge Date* field indicates when a key vault object will be permanently deleted, if no action is taken.</span></span> <span data-ttu-id="19a18-201">Som standard är hello kvarhållningsperiod för ett objekt för borttagna nyckelvalv 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="19a18-201">By default, hello retention period for a deleted key vault object is 90 days.</span></span>

>[!NOTE]
><span data-ttu-id="19a18-202">Ett objekt för borttagna valvet utlöses av dess *schemalagda Rensa datum* fältet, bort permanent.</span><span class="sxs-lookup"><span data-stu-id="19a18-202">A purged vault object, triggered by its *Scheduled Purge Date* field, is permanently deleted.</span></span> <span data-ttu-id="19a18-203">Den kan inte återställas.</span><span class="sxs-lookup"><span data-stu-id="19a18-203">It is not recoverable.</span></span>

## <a name="other-resources"></a><span data-ttu-id="19a18-204">Andra resurser</span><span class="sxs-lookup"><span data-stu-id="19a18-204">Other resources</span></span>

- <span data-ttu-id="19a18-205">En översikt över funktionen för mjuk borttagning av Key Vault finns [översikt över Azure Key Vault-soft-ta bort](key-vault-ovw-soft-delete.md).</span><span class="sxs-lookup"><span data-stu-id="19a18-205">For an overview of Key Vault's soft-delete feature, see [Azure Key Vault soft-delete overview](key-vault-ovw-soft-delete.md).</span></span>
- <span data-ttu-id="19a18-206">En allmän översikt över användning av Azure Key Vault finns [Kom igång med Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="19a18-206">For a general overview of Azure Key Vault usage, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>

