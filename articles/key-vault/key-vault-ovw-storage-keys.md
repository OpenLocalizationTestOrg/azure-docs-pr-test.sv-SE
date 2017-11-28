---
ms.assetid: 
title: Azure Key Vault Lagringskontonycklar
description: "Lagringskontonycklar ge en seemless integrering mellan Azure Key Vault och baserat åtkomst till Azure Storage-konto."
ms.topic: article
services: key-vault
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/25/2017
ms.openlocfilehash: 3148088c88236c64e089fd25c98eb8ac7cdcbfea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-key-vault-storage-account-keys"></a><span data-ttu-id="034ba-103">Azure Key Vault Lagringskontonycklar</span><span class="sxs-lookup"><span data-stu-id="034ba-103">Azure Key Vault Storage Account Keys</span></span>

<span data-ttu-id="034ba-104">Innan Azure Key Vault Lagringskontonycklar hade utvecklare att hantera sina egna nycklar för Azure Storage-konto (ASA) och rotera dem manuellt eller via en extern automation.</span><span class="sxs-lookup"><span data-stu-id="034ba-104">Before Azure Key Vault Storage Account Keys, developers had to manage their own Azure Storage Account (ASA) keys and rotate them manually or through an external automation.</span></span> <span data-ttu-id="034ba-105">Nu Key Vault Lagringskontonycklar implementeras som [Key Vault hemligheter](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) för att autentisera med Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="034ba-105">Now, Key Vault Storage Account Keys are implemented as [Key Vault secrets](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) for authenticating with an Azure Storage Account.</span></span> 

<span data-ttu-id="034ba-106">Funktionen ASA hanterar hemliga rotation du och tar bort behovet av din direkt kontakt med en ASA nyckel genom att erbjuda delad åtkomst signaturer (SAS) som en metod.</span><span class="sxs-lookup"><span data-stu-id="034ba-106">The ASA key feature manages secret rotation for you and removes the need for your direct contact with an ASA key by offering Shared Access Signatures (SAS) as a method.</span></span> 

<span data-ttu-id="034ba-107">Mer allmän information om Azure Storage-konton finns [om Azure storage-konton](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span><span class="sxs-lookup"><span data-stu-id="034ba-107">For more general information on Azure Storage Accounts, see [About Azure storage accounts](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span></span>

## <a name="supporting-interfaces"></a><span data-ttu-id="034ba-108">Stöder gränssnitt</span><span class="sxs-lookup"><span data-stu-id="034ba-108">Supporting interfaces</span></span>

<span data-ttu-id="034ba-109">Funktionen för nycklar i Azure Storage-konto är är tillgängliga via RESTEN .NET / C# och PowerShell-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="034ba-109">The Azure Storage Account keys feature is initially available through the REST, .NET/C# and PowerShell interfaces.</span></span> <span data-ttu-id="034ba-110">Mer information finns i [valvet nyckelreferens](https://docs.microsoft.com/azure/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="034ba-110">For more information, see [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).</span></span>


## <a name="storage-account-keys-behavior"></a><span data-ttu-id="034ba-111">Storage-konto nycklar beteende</span><span class="sxs-lookup"><span data-stu-id="034ba-111">Storage account keys behavior</span></span>

### <a name="what-key-vault-manages"></a><span data-ttu-id="034ba-112">Key Vault hanterar</span><span class="sxs-lookup"><span data-stu-id="034ba-112">What Key Vault manages</span></span>

<span data-ttu-id="034ba-113">Key Vault utför flera interna hanteringsfunktioner för din räkning när du använder nycklar för Lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="034ba-113">Key Vault performs several internal management functions on your behalf when you use Storage Account Keys.</span></span>

1. <span data-ttu-id="034ba-114">Azure Key Vault hanterar nycklar för ett Azure Storage konto (ASA).</span><span class="sxs-lookup"><span data-stu-id="034ba-114">Azure Key Vault manages keys of an Azure Storage Account (ASA).</span></span> 
    - <span data-ttu-id="034ba-115">Azure Key Vault kan internt, lista (sync) nycklar med ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="034ba-115">Internally, Azure Key Vault can list (sync) keys with an Azure Storage Account.</span></span>  
    - <span data-ttu-id="034ba-116">Azure Key Vault återskapar (roterar) nycklar med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="034ba-116">Azure Key Vault regenerates (rotates) the keys periodically.</span></span> 
    - <span data-ttu-id="034ba-117">Nyckelvärden returneras aldrig i svaret till anroparen.</span><span class="sxs-lookup"><span data-stu-id="034ba-117">Key values are never returned in response to caller.</span></span> 
    - <span data-ttu-id="034ba-118">Azure Key Vault hanterar nycklarna i både Storage-konton och klassiska Lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="034ba-118">Azure Key Vault manages keys of both Storage Accounts and Classic Storage Accounts.</span></span> 
2. <span data-ttu-id="034ba-119">Azure Key Vault kan du valvet/objektets ägare, att skapa definitioner för SAS (konto eller tjänst-SAS).</span><span class="sxs-lookup"><span data-stu-id="034ba-119">Azure Key Vault allows you, the vault/object owner, to create SAS (account or service SAS) definitions.</span></span> 
    - <span data-ttu-id="034ba-120">SAS-värdet som skapats med hjälp av SAS-definition, returneras som en hemlighet via REST-URI-sökväg.</span><span class="sxs-lookup"><span data-stu-id="034ba-120">The SAS value, created using SAS definition, is returned as a secret via the REST URI path.</span></span> <span data-ttu-id="034ba-121">Mer information finns i [Azure Key Vault lagringsåtgärder konto](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span><span class="sxs-lookup"><span data-stu-id="034ba-121">For more information, see [Azure Key Vault storage account operations](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span></span>

### <a name="naming-guidance"></a><span data-ttu-id="034ba-122">Riktlinjer för namngivning</span><span class="sxs-lookup"><span data-stu-id="034ba-122">Naming guidance</span></span>

<span data-ttu-id="034ba-123">Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="034ba-123">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span>  
 
<span data-ttu-id="034ba-124">Namnet på definitionen av en SAS måste vara 1 102 tecken som innehåller bara 0-9, a-z, A-Z.</span><span class="sxs-lookup"><span data-stu-id="034ba-124">A SAS definition name must be 1-102 characters in length containing only 0-9, a-z, A-Z.</span></span>

## <a name="developer-experience"></a><span data-ttu-id="034ba-125">Upplevelse för utvecklare</span><span class="sxs-lookup"><span data-stu-id="034ba-125">Developer experience</span></span> 

### <a name="before-azure-key-vault-storage-keys"></a><span data-ttu-id="034ba-126">Innan Azure Key Vault Lagringsnycklar</span><span class="sxs-lookup"><span data-stu-id="034ba-126">Before Azure Key Vault Storage Keys</span></span> 

<span data-ttu-id="034ba-127">Utvecklare används för att göra följande med en lagringskontonyckel att få åtkomst till Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="034ba-127">Developers used to need to do the following practices with a storage account key to get access to Azure storage.</span></span> 
 
 ```
//create storage account using connection string containing account name 
// and the storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a><span data-ttu-id="034ba-128">När Azure Key Vault Lagringsnycklar</span><span class="sxs-lookup"><span data-stu-id="034ba-128">After Azure Key Vault Storage Keys</span></span> 

```
//Please make sure to set storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Use PowerShell command to get Secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  
-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use credentials and the Blob storage endpoint to create a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If SAS token is about to expire then Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);

  ```
 
 ### <a name="developer-best-practices"></a><span data-ttu-id="034ba-129">Metodtips för utvecklare</span><span class="sxs-lookup"><span data-stu-id="034ba-129">Developer best practices</span></span> 

- <span data-ttu-id="034ba-130">Tillåt endast Key Vault att hantera ASA-nycklar.</span><span class="sxs-lookup"><span data-stu-id="034ba-130">Only allow Key Vault to manage your ASA keys.</span></span> <span data-ttu-id="034ba-131">Försök inte hantera dem själv, du stör Key Vault-processer.</span><span class="sxs-lookup"><span data-stu-id="034ba-131">Do not attempt to manage them yourself, you will interfere with Key Vault's processes.</span></span> 
- <span data-ttu-id="034ba-132">Tillåt inte ASA nycklar som ska hanteras av fler än ett Key Vault-objekt.</span><span class="sxs-lookup"><span data-stu-id="034ba-132">Do not allow ASA keys to be managed by more than one Key Vault object.</span></span> 
- <span data-ttu-id="034ba-133">Om du måste manuellt återskapa nycklarna ASA, rekommenderar vi att återskapa via Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="034ba-133">If you need to manually regenerate your ASA keys, we recommend that you regenerate them via Key Vault.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="034ba-134">Komma igång</span><span class="sxs-lookup"><span data-stu-id="034ba-134">Getting started</span></span>

### <a name="setup-for-role-based-access-control-rbac-permissions"></a><span data-ttu-id="034ba-135">Installationsprogrammet för rollbaserade behörigheter för åtkomstkontroll (RBAC)</span><span class="sxs-lookup"><span data-stu-id="034ba-135">Setup for role-based access control (RBAC) permissions</span></span>

<span data-ttu-id="034ba-136">Key Vault behöver behörighet att *lista* och *återskapa* nycklar för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="034ba-136">Key Vault needs permissions to *list* and *regenerate* keys for a storage account.</span></span> <span data-ttu-id="034ba-137">Ställa in behörigheterna med följande steg:</span><span class="sxs-lookup"><span data-stu-id="034ba-137">Set up these permissions using the following steps:</span></span>

- <span data-ttu-id="034ba-138">Hämta ObjectId för Key Vault:</span><span class="sxs-lookup"><span data-stu-id="034ba-138">Get ObjectId of Key Vault:</span></span> 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- <span data-ttu-id="034ba-139">Tilldela Azure Key Vault Identity operatörsrollen lagring nyckel:</span><span class="sxs-lookup"><span data-stu-id="034ba-139">Assign Storage Key Operator role to Azure Key Vault Identity:</span></span> 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > <span data-ttu-id="034ba-140">Ange parametern roll för klassiska kontotyp, *”klassiska Storage-konto nyckeln Service Operatörsrollen”*.</span><span class="sxs-lookup"><span data-stu-id="034ba-140">For a classic account type, set the role parameter to *"Classic Storage Account Key Operator Service Role"*.</span></span>

### <a name="storage-account-onboarding"></a><span data-ttu-id="034ba-141">Onboarding för Storage-konto</span><span class="sxs-lookup"><span data-stu-id="034ba-141">Storage account onboarding</span></span> 

<span data-ttu-id="034ba-142">Exempel: som en Key Vault objektägaren som du lägger till ett lagringsobjekt konto till din Azure Key Vault att publicera ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="034ba-142">Example: As a Key Vault object owner you add a storage account object to your Azure Key Vault to onboard a storage account.</span></span>

<span data-ttu-id="034ba-143">Under onboarding, Key Vault behöver kontrollera att identiteten för onboarding-tjänstkontot har behörighet att *lista* och *återskapa* lagringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="034ba-143">During onboarding, Key Vault needs to verify that the identity of the onboarding account has permissions to *list* and to *regenerate* storage keys.</span></span> <span data-ttu-id="034ba-144">För att kontrollera behörigheterna Key Vault får en OBO (på uppdrag av) token från Autentiseringstjänsten målgruppen som har angetts till Azure Resource Manager och gör en *lista* nyckeln anrop till Azure Storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="034ba-144">In order to verify these permissions, Key Vault gets an OBO (On Behalf Of) token from the authentication service, audience set to Azure Resource Manager, and makes a *list* key call to the Azure Storage service.</span></span> <span data-ttu-id="034ba-145">Om den *lista* anropet misslyckas Nyckelvalvet objektgenereringen misslyckas med en HTTP-statuskoden *förbjuden*.</span><span class="sxs-lookup"><span data-stu-id="034ba-145">If the *list* call fails, the Key Vault object creation fails with a HTTP status code of *Forbidden*.</span></span> <span data-ttu-id="034ba-146">Nycklarna som anges i det här sättet cachelagras med nyckelvalv entitet lagringen.</span><span class="sxs-lookup"><span data-stu-id="034ba-146">The keys listed in this fashion are cached with your key vault entity storage.</span></span> 

<span data-ttu-id="034ba-147">Key Vault måste kontrollera att identiteten har *återskapa* behörigheter innan den kan bli ägare till återskapar dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="034ba-147">Key Vault must verify that the identity has *regenerate* permissions before it can take ownership of regenerating your keys.</span></span> <span data-ttu-id="034ba-148">Att kontrollera att identiteten via OBO token som Key Vault första part identitet har följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="034ba-148">To verify that the identity, via OBO token, as well as the Key Vault first party identity has these permissions:</span></span>

- <span data-ttu-id="034ba-149">Key Vault visar RBAC-behörighet på resursen storage-konto.</span><span class="sxs-lookup"><span data-stu-id="034ba-149">Key Vault lists RBAC permissions on the storage account resource.</span></span>
- <span data-ttu-id="034ba-150">Key Vault verifierar svar via matchning med reguljära uttryck för och icke-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="034ba-150">Key Vault validates the response via regular expression matching of actions and non-actions.</span></span> 

<span data-ttu-id="034ba-151">Hitta exempel på [Key Vault - hanterade Storage-konto nycklar exempel](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span><span class="sxs-lookup"><span data-stu-id="034ba-151">Find some supporting examples at [Key Vault - Managed Storage Account Keys Samples](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span></span>

<span data-ttu-id="034ba-152">Om identiteten saknar *återskapa* behörigheter eller om Key Vault första part identitet inte har *lista* eller *återskapa* behörighet och sedan onboarding-processen för begäran misslyckas returnerar en felkod och ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="034ba-152">If the identity does not have *regenerate* permissions or if Key Vault's first party identity doesn’t have *list* or *regenerate* permission, then the onboarding request fails returning an appropriate error code and message.</span></span> 

<span data-ttu-id="034ba-153">OBO token fungerar endast när du använder från första part, interna klientprogram av PowerShell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="034ba-153">The OBO token will only work when you use first-party, native client applications of either PowerShell or CLI.</span></span>

## <a name="other-applications"></a><span data-ttu-id="034ba-154">Andra program</span><span class="sxs-lookup"><span data-stu-id="034ba-154">Other applications</span></span>

- <span data-ttu-id="034ba-155">SAS-token som skapats med hjälp av Key Vault lagringskontonycklar ger ännu mer kontrollerad åtkomst till ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="034ba-155">SAS tokens, constructed using Key Vault storage account keys, provide even more controlled access to an Azure storage account.</span></span> <span data-ttu-id="034ba-156">Mer information finns i [använder signaturer för delad åtkomst](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="034ba-156">For more information, see [Using shared access signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

## <a name="see-also"></a><span data-ttu-id="034ba-157">Se även</span><span class="sxs-lookup"><span data-stu-id="034ba-157">See also</span></span>

- [<span data-ttu-id="034ba-158">Om nycklar, hemligheter och certifikat</span><span class="sxs-lookup"><span data-stu-id="034ba-158">About keys, secrets, and certificates</span></span>](https://docs.microsoft.com/rest/api/keyvault/)
- [<span data-ttu-id="034ba-159">Key Vault-teamets blogg</span><span class="sxs-lookup"><span data-stu-id="034ba-159">Key Vault Team Blog</span></span>](https://blogs.technet.microsoft.com/kv/)
