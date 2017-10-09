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
# <a name="azure-key-vault-storage-account-keys"></a>Azure Key Vault Lagringskontonycklar

Innan Azure Key Vault Lagringskontonycklar, utvecklare hade toomanage sina egna nycklar för Azure Storage-konto (ASA) och rotera dem manuellt eller via en extern automation. Nu Key Vault Lagringskontonycklar implementeras som [Key Vault hemligheter](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) för att autentisera med Azure Storage-konto. 

hello ASA nyckelfunktion hanterar hemliga rotation du och tar bort hello behovet av din direkt kontakt med en ASA nyckel genom att erbjuda delad åtkomst signaturer (SAS) som en metod. 

Mer allmän information om Azure Storage-konton finns [om Azure storage-konton](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Stöder gränssnitt

hello Azure Storage-konto för funktionen är är tillgängliga via hello vila, .NET / C# och PowerShell-gränssnitt. Mer information finns i [valvet nyckelreferens](https://docs.microsoft.com/azure/key-vault/).


## <a name="storage-account-keys-behavior"></a>Storage-konto nycklar beteende

### <a name="what-key-vault-manages"></a>Key Vault hanterar

Key Vault utför flera interna hanteringsfunktioner för din räkning när du använder nycklar för Lagringskonto.

1. Azure Key Vault hanterar nycklar för ett Azure Storage konto (ASA). 
    - Azure Key Vault kan internt, lista (sync) nycklar med ett Azure Storage-konto.  
    - Azure Key Vault återskapar (roterar) hello nycklar med jämna mellanrum. 
    - Nyckelvärden returneras aldrig i svaret toocaller. 
    - Azure Key Vault hanterar nycklarna i både Storage-konton och klassiska Lagringskonton. 
2. Azure Key Vault kan du, hello valvet/objektets ägare, toocreate SAS (konto eller tjänst-SAS) definitioner. 
    - hello SAS-värde som skapats med hjälp av SAS-definition, returneras som en hemlighet via hello REST-URI-sökväg. Mer information finns i [Azure Key Vault lagringsåtgärder konto](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).

### <a name="naming-guidance"></a>Riktlinjer för namngivning

Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.  
 
Namnet på definitionen av en SAS måste vara 1 102 tecken som innehåller bara 0-9, a-z, A-Z.

## <a name="developer-experience"></a>Upplevelse för utvecklare 

### <a name="before-azure-key-vault-storage-keys"></a>Innan Azure Key Vault Lagringsnycklar 

Utvecklare används tooneed toodo hello följande metoder med en storage-konto viktiga tooget åtkomst tooAzure lagring. 
 
 ```
//create storage account using connection string containing account name 
// and hello storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>När Azure Key Vault Lagringsnycklar 

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
 
 ### <a name="developer-best-practices"></a>Metodtips för utvecklare 

- Tillåt endast Key Vault toomanage ASA-nycklar. Försök inte toomanage dem själv du stör Key Vault-processer. 
- Tillåt inte ASA nycklar toobe hanteras av fler än ett Key Vault-objekt. 
- Om du behöver toomanually återskapa nycklarna ASA rekommenderar vi att du återskapar dem via Key Vault. 

## <a name="getting-started"></a>Komma igång

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Installationsprogrammet för rollbaserade behörigheter för åtkomstkontroll (RBAC)

Key Vault behöver behörigheter för*lista* och *återskapa* nycklar för ett lagringskonto. Ställ in behörigheten med hello följande steg:

- Hämta ObjectId för Key Vault: 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- Tilldela lagring nyckeloperatorn rollen tooAzure Key Vault identitet: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > För klassiska kontotyp, ange hello rollen parametern för*”klassiska Storage-konto nyckeln Service Operatörsrollen”*.

### <a name="storage-account-onboarding"></a>Onboarding för Storage-konto 

Exempel: Som en Key Vault objektets ägare som du lägger till en lagringsplats för kontot objektet tooyour Azure Key Vault tooonboard ett lagringskonto.

Under onboarding, Key Vault måste tooverify hello identiteten för hello onboarding-tjänstkontot har behörighet för*lista* och för*återskapa* lagringsnycklar. Order tooverify behörigheterna, Key Vault hämtar en OBO (på uppdrag av) token från hello authentication service, målgrupp ange tooAzure Resource Manager och gör en *lista* nyckeln anropet toohello Azure Storage-tjänst. Om hello *lista* anropa misslyckas, hello Key Vault objektgenereringen misslyckas med en HTTP-statuskoden *förbjuden*. hello nycklar som anges i det här sättet cachelagras med nyckelvalv entitet lagringen. 

Key Vault måste kontrollera att hello identitet har *återskapa* behörigheter innan den kan bli ägare till återskapar dina nycklar. tooverify som hello identitet via OBO token, som även hello Key Vault första part identitet har följande behörigheter:

- Key Vault visar RBAC behörigheter för resurs för hello storage-konto.
- Key Vault verifierar hello svar via matchning med reguljära uttryck för och icke-åtgärder. 

Hitta exempel på [Key Vault - hanterade Storage-konto nycklar exempel](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Om hello identitet saknar *återskapa* behörigheter eller om Key Vault första part identitet inte har *lista* eller *återskapa* behörighet och sedan hello onboarding begäran misslyckas returnerar en felkod och ett meddelande. 

Hej OBO token fungerar endast när du använder från första part, interna klientprogram av PowerShell eller CLI.

## <a name="other-applications"></a>Andra program

- SAS-token som skapats med hjälp av Key Vault lagringskontonycklar, ange ännu mer kontrollerad åtkomst tooan Azure storage-konto. Mer information finns i [använder signaturer för delad åtkomst](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Se även

- [Om nycklar, hemligheter och certifikat](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault-teamets blogg](https://blogs.technet.microsoft.com/kv/)
