---
ms.assetid: 
title: aaaAzure Key Vault Lagringskontonycklar
description: "Lagringskontonycklar ger en seemless integrering mellan Azure Key Vault och baserat åtkomst tooAzure Storage-konto."
ms.topic: article
services: key-vault
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/25/2017
ms.openlocfilehash: becdf97798a08164c48d3a7a14aea6ca54085c9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-storage-account-keys"></a><span data-ttu-id="a71ec-103">Azure Key Vault Lagringskontonycklar</span><span class="sxs-lookup"><span data-stu-id="a71ec-103">Azure Key Vault Storage Account Keys</span></span>

<span data-ttu-id="a71ec-104">Innan Azure Key Vault Lagringskontonycklar, utvecklare hade toomanage sina egna nycklar för Azure Storage-konto (ASA) och rotera dem manuellt eller via en extern automation.</span><span class="sxs-lookup"><span data-stu-id="a71ec-104">Before Azure Key Vault Storage Account Keys, developers had toomanage their own Azure Storage Account (ASA) keys and rotate them manually or through an external automation.</span></span> <span data-ttu-id="a71ec-105">Nu Key Vault Lagringskontonycklar implementeras som [Key Vault hemligheter](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) för att autentisera med Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a71ec-105">Now, Key Vault Storage Account Keys are implemented as [Key Vault secrets](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) for authenticating with an Azure Storage Account.</span></span> 

<span data-ttu-id="a71ec-106">hello ASA nyckelfunktion hanterar hemliga rotation du och tar bort hello behovet av din direkt kontakt med en ASA nyckel genom att erbjuda delad åtkomst signaturer (SAS) som en metod.</span><span class="sxs-lookup"><span data-stu-id="a71ec-106">hello ASA key feature manages secret rotation for you and removes hello need for your direct contact with an ASA key by offering Shared Access Signatures (SAS) as a method.</span></span> 

<span data-ttu-id="a71ec-107">Mer allmän information om Azure Storage-konton finns [om Azure storage-konton](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span><span class="sxs-lookup"><span data-stu-id="a71ec-107">For more general information on Azure Storage Accounts, see [About Azure storage accounts](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</span></span>

## <a name="supporting-interfaces"></a><span data-ttu-id="a71ec-108">Stöder gränssnitt</span><span class="sxs-lookup"><span data-stu-id="a71ec-108">Supporting interfaces</span></span>

<span data-ttu-id="a71ec-109">hello Azure Storage-konto för funktionen är är tillgängliga via hello vila, .NET / C# och PowerShell-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="a71ec-109">hello Azure Storage Account keys feature is initially available through hello REST, .NET/C# and PowerShell interfaces.</span></span> <span data-ttu-id="a71ec-110">Mer information finns i [valvet nyckelreferens](https://docs.microsoft.com/azure/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="a71ec-110">For more information, see [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).</span></span>


## <a name="storage-account-keys-behavior"></a><span data-ttu-id="a71ec-111">Storage-konto nycklar beteende</span><span class="sxs-lookup"><span data-stu-id="a71ec-111">Storage account keys behavior</span></span>

### <a name="what-key-vault-manages"></a><span data-ttu-id="a71ec-112">Key Vault hanterar</span><span class="sxs-lookup"><span data-stu-id="a71ec-112">What Key Vault manages</span></span>

<span data-ttu-id="a71ec-113">Key Vault utför flera interna hanteringsfunktioner för din räkning när du använder nycklar för Lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a71ec-113">Key Vault performs several internal management functions on your behalf when you use Storage Account Keys.</span></span>

1. <span data-ttu-id="a71ec-114">Azure Key Vault hanterar nycklar för ett Azure Storage konto (ASA).</span><span class="sxs-lookup"><span data-stu-id="a71ec-114">Azure Key Vault manages keys of an Azure Storage Account (ASA).</span></span> 
    - <span data-ttu-id="a71ec-115">Azure Key Vault kan internt, lista (sync) nycklar med ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a71ec-115">Internally, Azure Key Vault can list (sync) keys with an Azure Storage Account.</span></span>  
    - <span data-ttu-id="a71ec-116">Azure Key Vault återskapar (roterar) hello nycklar med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="a71ec-116">Azure Key Vault regenerates (rotates) hello keys periodically.</span></span> 
    - <span data-ttu-id="a71ec-117">Nyckelvärden returneras aldrig i svaret toocaller.</span><span class="sxs-lookup"><span data-stu-id="a71ec-117">Key values are never returned in response toocaller.</span></span> 
    - <span data-ttu-id="a71ec-118">Azure Key Vault hanterar nycklarna i både Storage-konton och klassiska Lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="a71ec-118">Azure Key Vault manages keys of both Storage Accounts and Classic Storage Accounts.</span></span> 
2. <span data-ttu-id="a71ec-119">Azure Key Vault kan du, hello valvet/objektets ägare, toocreate SAS (konto eller tjänst-SAS) definitioner.</span><span class="sxs-lookup"><span data-stu-id="a71ec-119">Azure Key Vault allows you, hello vault/object owner, toocreate SAS (account or service SAS) definitions.</span></span> 
    - <span data-ttu-id="a71ec-120">hello SAS-värde som skapats med hjälp av SAS-definition, returneras som en hemlighet via hello REST-URI-sökväg.</span><span class="sxs-lookup"><span data-stu-id="a71ec-120">hello SAS value, created using SAS definition, is returned as a secret via hello REST URI path.</span></span> <span data-ttu-id="a71ec-121">Mer information finns i [Azure Key Vault lagringsåtgärder konto](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span><span class="sxs-lookup"><span data-stu-id="a71ec-121">For more information, see [Azure Key Vault storage account operations](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).</span></span>

### <a name="naming-guidance"></a><span data-ttu-id="a71ec-122">Riktlinjer för namngivning</span><span class="sxs-lookup"><span data-stu-id="a71ec-122">Naming guidance</span></span>

<span data-ttu-id="a71ec-123">Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="a71ec-123">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span>  
 
<span data-ttu-id="a71ec-124">Namnet på definitionen av en SAS måste vara 1 102 tecken som innehåller bara 0-9, a-z, A-Z.</span><span class="sxs-lookup"><span data-stu-id="a71ec-124">A SAS definition name must be 1-102 characters in length containing only 0-9, a-z, A-Z.</span></span>

## <a name="developer-experience"></a><span data-ttu-id="a71ec-125">Upplevelse för utvecklare</span><span class="sxs-lookup"><span data-stu-id="a71ec-125">Developer experience</span></span> 

### <a name="before-azure-key-vault-storage-keys"></a><span data-ttu-id="a71ec-126">Innan Azure Key Vault Lagringsnycklar</span><span class="sxs-lookup"><span data-stu-id="a71ec-126">Before Azure Key Vault Storage Keys</span></span> 

<span data-ttu-id="a71ec-127">Utvecklare används tooneed toodo hello följande metoder med en storage-konto viktiga tooget åtkomst tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="a71ec-127">Developers used tooneed toodo hello following practices with a storage account key tooget access tooAzure storage.</span></span> 
 
 ```
//create storage account using connection string containing account name 
// and hello storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a><span data-ttu-id="a71ec-128">När Azure Key Vault Lagringsnycklar</span><span class="sxs-lookup"><span data-stu-id="a71ec-128">After Azure Key Vault Storage Keys</span></span> 

```
//Please make sure tooset storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Use PowerShell command tooget Secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  
-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using hello SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use credentials and hello Blob storage endpoint toocreate a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If SAS token is about tooexpire then Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);

  ```
 
 ### <a name="developer-best-practices"></a><span data-ttu-id="a71ec-129">Metodtips för utvecklare</span><span class="sxs-lookup"><span data-stu-id="a71ec-129">Developer best practices</span></span> 

- <span data-ttu-id="a71ec-130">Tillåt endast Key Vault toomanage ASA-nycklar.</span><span class="sxs-lookup"><span data-stu-id="a71ec-130">Only allow Key Vault toomanage your ASA keys.</span></span> <span data-ttu-id="a71ec-131">Försök inte toomanage dem själv du stör Key Vault-processer.</span><span class="sxs-lookup"><span data-stu-id="a71ec-131">Do not attempt toomanage them yourself, you will interfere with Key Vault's processes.</span></span> 
- <span data-ttu-id="a71ec-132">Tillåt inte ASA nycklar toobe hanteras av fler än ett Key Vault-objekt.</span><span class="sxs-lookup"><span data-stu-id="a71ec-132">Do not allow ASA keys toobe managed by more than one Key Vault object.</span></span> 
- <span data-ttu-id="a71ec-133">Om du behöver toomanually återskapa nycklarna ASA rekommenderar vi att du återskapar dem via Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a71ec-133">If you need toomanually regenerate your ASA keys, we recommend that you regenerate them via Key Vault.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="a71ec-134">Komma igång</span><span class="sxs-lookup"><span data-stu-id="a71ec-134">Getting started</span></span>

### <a name="setup-for-role-based-access-control-rbac-permissions"></a><span data-ttu-id="a71ec-135">Installationsprogrammet för rollbaserade behörigheter för åtkomstkontroll (RBAC)</span><span class="sxs-lookup"><span data-stu-id="a71ec-135">Setup for role-based access control (RBAC) permissions</span></span>

<span data-ttu-id="a71ec-136">Key Vault behöver behörigheter för*lista* och *återskapa* nycklar för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a71ec-136">Key Vault needs permissions too*list* and *regenerate* keys for a storage account.</span></span> <span data-ttu-id="a71ec-137">Ställ in behörigheten med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a71ec-137">Set up these permissions using hello following steps:</span></span>

- <span data-ttu-id="a71ec-138">Hämta ObjectId för Key Vault:</span><span class="sxs-lookup"><span data-stu-id="a71ec-138">Get ObjectId of Key Vault:</span></span> 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- <span data-ttu-id="a71ec-139">Tilldela lagring nyckeloperatorn rollen tooAzure Key Vault identitet:</span><span class="sxs-lookup"><span data-stu-id="a71ec-139">Assign Storage Key Operator role tooAzure Key Vault Identity:</span></span> 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > <span data-ttu-id="a71ec-140">För klassiska kontotyp, ange hello rollen parametern för*”klassiska Storage-konto nyckeln Service Operatörsrollen”*.</span><span class="sxs-lookup"><span data-stu-id="a71ec-140">For a classic account type, set hello role parameter too*"Classic Storage Account Key Operator Service Role"*.</span></span>

### <a name="storage-account-onboarding"></a><span data-ttu-id="a71ec-141">Onboarding för Storage-konto</span><span class="sxs-lookup"><span data-stu-id="a71ec-141">Storage account onboarding</span></span> 

<span data-ttu-id="a71ec-142">Exempel: Som en Key Vault objektets ägare som du lägger till en lagringsplats för kontot objektet tooyour Azure Key Vault tooonboard ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a71ec-142">Example: As a Key Vault object owner you add a storage account object tooyour Azure Key Vault tooonboard a storage account.</span></span>

<span data-ttu-id="a71ec-143">Under onboarding, Key Vault måste tooverify hello identiteten för hello onboarding-tjänstkontot har behörighet för*lista* och för*återskapa* lagringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="a71ec-143">During onboarding, Key Vault needs tooverify that hello identity of hello onboarding account has permissions too*list* and too*regenerate* storage keys.</span></span> <span data-ttu-id="a71ec-144">Order tooverify behörigheterna, Key Vault hämtar en OBO (på uppdrag av) token från hello authentication service, målgrupp ange tooAzure Resource Manager och gör en *lista* nyckeln anropet toohello Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="a71ec-144">In order tooverify these permissions, Key Vault gets an OBO (On Behalf Of) token from hello authentication service, audience set tooAzure Resource Manager, and makes a *list* key call toohello Azure Storage service.</span></span> <span data-ttu-id="a71ec-145">Om hello *lista* anropa misslyckas, hello Key Vault objektgenereringen misslyckas med en HTTP-statuskoden *förbjuden*.</span><span class="sxs-lookup"><span data-stu-id="a71ec-145">If hello *list* call fails, hello Key Vault object creation fails with a HTTP status code of *Forbidden*.</span></span> <span data-ttu-id="a71ec-146">hello nycklar som anges i det här sättet cachelagras med nyckelvalv entitet lagringen.</span><span class="sxs-lookup"><span data-stu-id="a71ec-146">hello keys listed in this fashion are cached with your key vault entity storage.</span></span> 

<span data-ttu-id="a71ec-147">Key Vault måste kontrollera att hello identitet har *återskapa* behörigheter innan den kan bli ägare till återskapar dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="a71ec-147">Key Vault must verify that hello identity has *regenerate* permissions before it can take ownership of regenerating your keys.</span></span> <span data-ttu-id="a71ec-148">tooverify som hello identitet via OBO token, som även hello Key Vault första part identitet har följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="a71ec-148">tooverify that hello identity, via OBO token, as well as hello Key Vault first party identity has these permissions:</span></span>

- <span data-ttu-id="a71ec-149">Key Vault visar RBAC behörigheter för resurs för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a71ec-149">Key Vault lists RBAC permissions on hello storage account resource.</span></span>
- <span data-ttu-id="a71ec-150">Key Vault verifierar hello svar via matchning med reguljära uttryck för och icke-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a71ec-150">Key Vault validates hello response via regular expression matching of actions and non-actions.</span></span> 

<span data-ttu-id="a71ec-151">Hitta exempel på [Key Vault - hanterade Storage-konto nycklar exempel](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span><span class="sxs-lookup"><span data-stu-id="a71ec-151">Find some supporting examples at [Key Vault - Managed Storage Account Keys Samples](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).</span></span>

<span data-ttu-id="a71ec-152">Om hello identitet saknar *återskapa* behörigheter eller om Key Vault första part identitet inte har *lista* eller *återskapa* behörighet och sedan hello onboarding begäran misslyckas returnerar en felkod och ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="a71ec-152">If hello identity does not have *regenerate* permissions or if Key Vault's first party identity doesn’t have *list* or *regenerate* permission, then hello onboarding request fails returning an appropriate error code and message.</span></span> 

<span data-ttu-id="a71ec-153">Hej OBO token fungerar endast när du använder från första part, interna klientprogram av PowerShell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="a71ec-153">hello OBO token will only work when you use first-party, native client applications of either PowerShell or CLI.</span></span>

## <a name="other-applications"></a><span data-ttu-id="a71ec-154">Andra program</span><span class="sxs-lookup"><span data-stu-id="a71ec-154">Other applications</span></span>

- <span data-ttu-id="a71ec-155">SAS-token som skapats med hjälp av Key Vault lagringskontonycklar, ange ännu mer kontrollerad åtkomst tooan Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a71ec-155">SAS tokens, constructed using Key Vault storage account keys, provide even more controlled access tooan Azure storage account.</span></span> <span data-ttu-id="a71ec-156">Mer information finns i [använder signaturer för delad åtkomst](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="a71ec-156">For more information, see [Using shared access signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

## <a name="see-also"></a><span data-ttu-id="a71ec-157">Se även</span><span class="sxs-lookup"><span data-stu-id="a71ec-157">See also</span></span>

- [<span data-ttu-id="a71ec-158">Om nycklar, hemligheter och certifikat</span><span class="sxs-lookup"><span data-stu-id="a71ec-158">About keys, secrets, and certificates</span></span>](https://docs.microsoft.com/rest/api/keyvault/)
- [<span data-ttu-id="a71ec-159">Key Vault-teamets blogg</span><span class="sxs-lookup"><span data-stu-id="a71ec-159">Key Vault Team Blog</span></span>](https://blogs.technet.microsoft.com/kv/)
