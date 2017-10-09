---
title: "Självstudier: Kryptera och dekryptera blobbar i Azure Storage med hjälp av Azure Key Vault | Microsoft Docs"
description: "Hur tooencrypt och dekryptera en blob med kryptering på klientsidan för Microsoft Azure Storage med Azure Key Vault."
services: storage
documentationcenter: 
author: adhurwit
manager: jasonsav
editor: tysonn
ms.assetid: 027e8631-c1bf-48c1-9d9b-f6843e88b583
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 01/23/2017
ms.author: adhurwit
ms.openlocfilehash: 3eb64f104f378dd09ef295c94e03167655883391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Självstudier: Kryptera och dekryptera blobbar i Microsoft Azure Storage med hjälp av Azure Key Vault
## <a name="introduction"></a>Introduktion
Den här självstudiekursen beskrivs hur toomake användning av klientsidan lagringskryptering med Azure Key Vault. Den vägleder dig igenom hur tooencrypt och dekryptera en blobb i ett konsolprogram med hjälp av dessa tekniker.

**Uppskattad tid toocomplete:** 20 minuter

Mer information om Azure Key Vault finns [vad är Azure Key Vault?](../key-vault/key-vault-whatis.md).

Mer information om klientens kryptering för Azure Storage finns [kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](storage-client-side-encryption.md).

## <a name="prerequisites"></a>Krav
toocomplete den här självstudien måste du ha hello följande:

* Ett Azure Storage-konto
* Visual Studio 2013 eller senare
* Azure PowerShell

## <a name="overview-of-client-side-encryption"></a>Översikt över kryptering på klientsidan
En översikt av klientsidan kryptering för Azure Storage finns [kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](storage-client-side-encryption.md)

Här är en kort beskrivning av hur kryptering på serversidan för klienten fungerar:

1. hello Azure Storage client SDK genererar en innehåll krypteringsnyckel (CEK), vilket är en gång Använd symmetrisk nyckel.
2. Kundinformation krypteras med den här CEK.
3. Hej kapslas CEK sedan (krypterade) med hjälp av krypteringsnyckeln för hello nyckel (KEK). Hej KEK identifieras med en nyckelidentifierare och kan vara ett asymmetriskt nyckelpar eller en symmetrisk nyckel och kan hanteras lokalt eller lagras i Azure Key Vault. hello lagring själva klienten har aldrig åtkomst toohello KEK. Den anropar bara hello viktiga radbrytning algoritmen som tillhandahålls av Key Vault. Kunder kan välja toouse anpassade providers för nyckeln radbrytning/uppackning om de vill.
4. hello krypterade data har sedan överföra toohello Azure Storage-tjänst.

## <a name="set-up-your-azure-key-vault"></a>Konfigurera Azure Key Vault
I ordning tooproceed med den här självstudiekursen, behöver du toodo hello följa stegen som beskrivs i hello självstudiekursen [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md):

* Skapa en nyckelvalvet.
* Lägg till en nyckel eller Hemlig toohello nyckelvalvet.
* Registrera ett program med Azure Active Directory.
* Auktorisera hello programmet toouse hello nyckel eller hemlighet.

Se anteckna hello ClientID och ClientSecret som genererats när du registrerar ett program med Azure Active Directory.

Skapa båda nycklarna i hello nyckelvalvet. Vi förutsätter hello resten av hello kursen att du har använt hello följande namn: ContosoKeyVault och TestRSAKey1.

## <a name="create-a-console-application-with-packages-and-appsettings"></a>Skapa ett konsolprogram med paket och AppSettings
Skapa ett nytt konsolprogram i Visual Studio.

Lägg till nödvändiga nuget-paket i hello Package Manager-konsolen.

```
Install-Package WindowsAzure.Storage

// This is hello latest stable release for ADAL.
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

Lägg till AppSettings toohello App.Config.

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

Lägg till följande hello `using` -satser och se till att tooadd referens tooSystem.Configuration toohello projektet.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Configuration;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.Azure.KeyVault;
using System.Threading;        
using System.IO;
```

## <a name="add-a-method-tooget-a-token-tooyour-console-application"></a>Lägg till en metod tooget ett token tooyour konsolprogram
hello används följande metod av Key Vault-klasser som behöver tooauthenticate för åtkomst tooyour nyckelvalvet.

```csharp
private async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(
        ConfigurationManager.AppSettings["clientId"],
        ConfigurationManager.AppSettings["clientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed tooobtain hello JWT token");

    return result.AccessToken;
}
```

## <a name="access-storage-and-key-vault-in-your-program"></a>Komma åt lagringsutrymme och Key Vault i ditt program
Lägg till följande kod hello i hello huvudfunktionen.

```csharp
// This is standard code toointeract with Blob storage.
StorageCredentials creds = new StorageCredentials(
    ConfigurationManager.AppSettings["accountName"],
       ConfigurationManager.AppSettings["accountKey"]);
CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
CloudBlobClient client = account.CreateCloudBlobClient();
CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
contain.CreateIfNotExists();

// hello Resolver object is used toointeract with Key Vault for Azure Storage.
// This is where hello GetToken method from above is used.
KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);
```

> [!NOTE]
> Key Vault Nätverksobjektmodellerna
> 
> Det är viktigt toounderstand att det finns faktiskt två Key Vault objektet modeller toobe medveten om: en är baserad på hello REST-API (KeyVault namnområde) och hello andra är ett tillägg för kryptering på klientsidan.
> 
> hello Key Vault klienten samverkar med hello REST-API och förstår JSON Web nycklar och hemligheter för hello två typer av saker som ingår i Nyckelvalvet.
> 
> hello Key Vault tillägg är klasser som verkar vara skapats specifikt för klientsidan kryptering i Azure Storage. De innehåller ett gränssnitt för nycklar (IKey) och klasser som baseras på hello konceptet för en nyckel-matchare. Det finns två implementeringar av du behöver tooknow IKey: RSAKey och SymmetricKey. Nu de råkar toocoincide med hello saker som ingår i ett Nyckelvalv, men då de är oberoende klasser (så inte implementerar IKey hello nyckel och Hemlig hämtas av hello Key Vault-klienten).
> 
> 

## <a name="encrypt-blob-and-upload"></a>Kryptera blob och ladda upp
Lägg till följande hello code tooencrypt en blobb och överför den tooyour Azure storage-konto. Hej **ResolveKeyAsync** returnerar metoden som används för en IKey.

```csharp
// Retrieve hello key that you created previously.
// hello IKey that is returned here is an RsaKey.
// Remember that we used hello names contosokeyvault and testrsakey1.
var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();

// Now you simply use hello RSA key tooencrypt by setting it in hello BlobEncryptionPolicy.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

// Reference a block blob.
CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

// Upload using hello UploadFromStream method.
using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
    blob.UploadFromStream(stream, stream.Length, null, options, null);
```

Följande är en skärmbild från hello [klassiska Azure-portalen](https://manage.windowsazure.com) för en blob som har krypterats med hjälp av klientsidan kryptering med en nyckel som lagras i Nyckelvalvet. Hej **KeyId** egenskapen är hello URI för hello nyckeln i Nyckelvalvet som fungerar som hello KEK. Hej **EncryptedKey** egenskap innehåller hello krypterad version av hello CEK.

![Skärmbild som visar Blob-metadata som innehåller krypteringsmetadata](./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png)

> [!NOTE]
> Om du tittar på hello BlobEncryptionPolicy konstruktorn visas att den kan acceptera en nyckel och/eller en DNS-matchare. Tänk som nu har du inte använda en DNS-matchare för kryptering eftersom den har för närvarande inte stöder en standard-nyckel.
> 
> 

## <a name="decrypt-blob-and-download"></a>Dekryptera blob och ladda ned
Dekryptering är verkligen när med hello matcharen vits med klasser. hello-ID för hello-nyckel som används för kryptering är associerad med hello blob i dess metadata, så det finns ingen anledning att du tooretrieve hello nyckel och Kom ihåg hello kopplingen mellan nyckel och blob. Du har precis toomake att hello nyckeln finns kvar i Nyckelvalvet.   

hello privata nyckeln för en RSA-nyckel finns kvar i Nyckelvalvet, så för dekryptering toooccur hello krypterade nyckeln från hello blobbmetadata som innehåller hello CEK skickas tooKey valvet för dekryptering.

Lägg till följande toodecrypt hello blob som du just har överfört hello.

```csharp
// In this case, we will not pass a key and only pass hello resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> Det finns några andra typer av matchare toomake nyckelhantering enklare, inklusive: AggregateKeyResolver och CachingKeyResolver.
> 
> 

## <a name="use-key-vault-secrets"></a>Använd Key Vault hemligheter
hello sätt toouse en hemlighet med kryptering på klientsidan är via hello SymmetricKey klassen eftersom en hemlighet är i grunden en symmetrisk nyckel. Men som nämns ovan, mappar en hemlighet i Nyckelvalvet inte exakt tooa SymmetricKey. Det finns några saker toounderstand:

* hello nyckeln i en SymmetricKey har toobe en fast längd: 128, 192, 256, 384 eller 512 bitar.
* hello nyckeln i en SymmetricKey måste vara Base64-kodad.
* En hemlighet i Nyckelvalvet som ska användas som en SymmetricKey måste toohave innehållstypen för ”program/oktettsröm” i Nyckelvalvet.

Här är ett exempel i PowerShell för att skapa en hemlighet i Nyckelvalvet som kan användas som en SymmetricKey.
Observera att hello hårda kodat värde, $key, för demonstration ändamål endast. I din kod ska du toogenerate den här nyckeln.

```csharp
// Here we are making a 128-bit key so we have 16 characters.
//     hello characters are in hello ASCII range of UTF8 so they are
//    each 1 byte. 16 x 8 = 128.
$key = "qwertyuiopasdfgh"
$b = [System.Text.Encoding]::UTF8.GetBytes($key)
$enc = [System.Convert]::ToBase64String($b)
$secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

// Substitute hello VaultName and Name in this command.
$secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"
```

Du kan använda samma anropa som före tooretrieve detta hemliga som en SymmetricKey hello i din konsolprogram.

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
Det var allt. Ha det så kul!

## <a name="next-steps"></a>Nästa steg
Mer information om hur du använder Microsoft Azure Storage med C# finns [Microsoft Azure Storage-klientbibliotek för .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Mer information om hello Blob REST-API finns [REST-API för Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Hello senaste informationen om Microsoft Azure Storage, gå toohello [Microsoft Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/).
