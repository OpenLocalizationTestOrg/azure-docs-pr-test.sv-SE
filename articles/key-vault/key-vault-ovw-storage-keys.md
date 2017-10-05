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
# <a name="azure-key-vault-storage-account-keys"></a>Azure Key Vault Lagringskontonycklar

Innan Azure Key Vault Lagringskontonycklar hade utvecklare att hantera sina egna nycklar för Azure Storage-konto (ASA) och rotera dem manuellt eller via en extern automation. Nu Key Vault Lagringskontonycklar implementeras som [Key Vault hemligheter](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) för att autentisera med Azure Storage-konto. 

Funktionen ASA hanterar hemliga rotation du och tar bort behovet av din direkt kontakt med en ASA nyckel genom att erbjuda delad åtkomst signaturer (SAS) som en metod. 

Mer allmän information om Azure Storage-konton finns [om Azure storage-konton](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Stöder gränssnitt

Funktionen för nycklar i Azure Storage-konto är är tillgängliga via RESTEN .NET / C# och PowerShell-gränssnitt. Mer information finns i [valvet nyckelreferens](https://docs.microsoft.com/azure/key-vault/).


## <a name="storage-account-keys-behavior"></a>Storage-konto nycklar beteende

### <a name="what-key-vault-manages"></a>Key Vault hanterar

Key Vault utför flera interna hanteringsfunktioner för din räkning när du använder nycklar för Lagringskonto.

1. Azure Key Vault hanterar nycklar för ett Azure Storage konto (ASA). 
    - Azure Key Vault kan internt, lista (sync) nycklar med ett Azure Storage-konto.  
    - Azure Key Vault återskapar (roterar) nycklar med jämna mellanrum. 
    - Nyckelvärden returneras aldrig i svaret till anroparen. 
    - Azure Key Vault hanterar nycklarna i både Storage-konton och klassiska Lagringskonton. 
2. Azure Key Vault kan du valvet/objektets ägare, att skapa definitioner för SAS (konto eller tjänst-SAS). 
    - SAS-värdet som skapats med hjälp av SAS-definition, returneras som en hemlighet via REST-URI-sökväg. Mer information finns i [Azure Key Vault lagringsåtgärder konto](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).

### <a name="naming-guidance"></a>Riktlinjer för namngivning

Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.  
 
Namnet på definitionen av en SAS måste vara 1 102 tecken som innehåller bara 0-9, a-z, A-Z.

## <a name="developer-experience"></a>Upplevelse för utvecklare 

### <a name="before-azure-key-vault-storage-keys"></a>Innan Azure Key Vault Lagringsnycklar 

Utvecklare används för att göra följande med en lagringskontonyckel att få åtkomst till Azure-lagring. 
 
 ```
//create storage account using connection string containing account name 
// and the storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>När Azure Key Vault Lagringsnycklar 

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
 
 ### <a name="developer-best-practices"></a>Metodtips för utvecklare 

- Tillåt endast Key Vault att hantera ASA-nycklar. Försök inte hantera dem själv, du stör Key Vault-processer. 
- Tillåt inte ASA nycklar som ska hanteras av fler än ett Key Vault-objekt. 
- Om du måste manuellt återskapa nycklarna ASA, rekommenderar vi att återskapa via Nyckelvalvet. 

## <a name="getting-started"></a>Komma igång

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Installationsprogrammet för rollbaserade behörigheter för åtkomstkontroll (RBAC)

Key Vault behöver behörighet att *lista* och *återskapa* nycklar för ett lagringskonto. Ställa in behörigheterna med följande steg:

- Hämta ObjectId för Key Vault: 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- Tilldela Azure Key Vault Identity operatörsrollen lagring nyckel: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > Ange parametern roll för klassiska kontotyp, *”klassiska Storage-konto nyckeln Service Operatörsrollen”*.

### <a name="storage-account-onboarding"></a>Onboarding för Storage-konto 

Exempel: som en Key Vault objektägaren som du lägger till ett lagringsobjekt konto till din Azure Key Vault att publicera ett lagringskonto.

Under onboarding, Key Vault behöver kontrollera att identiteten för onboarding-tjänstkontot har behörighet att *lista* och *återskapa* lagringsnycklar. För att kontrollera behörigheterna Key Vault får en OBO (på uppdrag av) token från Autentiseringstjänsten målgruppen som har angetts till Azure Resource Manager och gör en *lista* nyckeln anrop till Azure Storage-tjänsten. Om den *lista* anropet misslyckas Nyckelvalvet objektgenereringen misslyckas med en HTTP-statuskoden *förbjuden*. Nycklarna som anges i det här sättet cachelagras med nyckelvalv entitet lagringen. 

Key Vault måste kontrollera att identiteten har *återskapa* behörigheter innan den kan bli ägare till återskapar dina nycklar. Att kontrollera att identiteten via OBO token som Key Vault första part identitet har följande behörigheter:

- Key Vault visar RBAC-behörighet på resursen storage-konto.
- Key Vault verifierar svar via matchning med reguljära uttryck för och icke-åtgärder. 

Hitta exempel på [Key Vault - hanterade Storage-konto nycklar exempel](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Om identiteten saknar *återskapa* behörigheter eller om Key Vault första part identitet inte har *lista* eller *återskapa* behörighet och sedan onboarding-processen för begäran misslyckas returnerar en felkod och ett meddelande. 

OBO token fungerar endast när du använder från första part, interna klientprogram av PowerShell eller CLI.

## <a name="other-applications"></a>Andra program

- SAS-token som skapats med hjälp av Key Vault lagringskontonycklar ger ännu mer kontrollerad åtkomst till ett Azure storage-konto. Mer information finns i [använder signaturer för delad åtkomst](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Se även

- [Om nycklar, hemligheter och certifikat](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault-teamets blogg](https://blogs.technet.microsoft.com/kv/)
