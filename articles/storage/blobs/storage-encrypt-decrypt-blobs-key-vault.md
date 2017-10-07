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
ms.openlocfilehash: e387dd419a51b5b1df62d10ead97268e8295ff56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a><span data-ttu-id="128c1-103">Självstudier: Kryptera och dekryptera blobbar i Microsoft Azure Storage med hjälp av Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="128c1-103">Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault</span></span>
## <a name="introduction"></a><span data-ttu-id="128c1-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="128c1-104">Introduction</span></span>
<span data-ttu-id="128c1-105">Den här självstudiekursen beskrivs hur toomake användning av klientsidan lagringskryptering med Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="128c1-105">This tutorial covers how toomake use of client-side storage encryption with Azure Key Vault.</span></span> <span data-ttu-id="128c1-106">Den vägleder dig igenom hur tooencrypt och dekryptera en blobb i ett konsolprogram med hjälp av dessa tekniker.</span><span class="sxs-lookup"><span data-stu-id="128c1-106">It walks you through how tooencrypt and decrypt a blob in a console application using these technologies.</span></span>

<span data-ttu-id="128c1-107">**Uppskattad tid toocomplete:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="128c1-107">**Estimated time toocomplete:** 20 minutes</span></span>

<span data-ttu-id="128c1-108">Mer information om Azure Key Vault finns [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="128c1-108">For overview information about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md).</span></span>

<span data-ttu-id="128c1-109">Mer information om klientens kryptering för Azure Storage finns [kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="128c1-109">For overview information about client-side encryption for Azure Storage, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="128c1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="128c1-110">Prerequisites</span></span>
<span data-ttu-id="128c1-111">toocomplete den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="128c1-111">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="128c1-112">Ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="128c1-112">An Azure Storage account</span></span>
* <span data-ttu-id="128c1-113">Visual Studio 2013 eller senare</span><span class="sxs-lookup"><span data-stu-id="128c1-113">Visual Studio 2013 or later</span></span>
* <span data-ttu-id="128c1-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="128c1-114">Azure PowerShell</span></span>

## <a name="overview-of-client-side-encryption"></a><span data-ttu-id="128c1-115">Översikt över kryptering på klientsidan</span><span class="sxs-lookup"><span data-stu-id="128c1-115">Overview of client-side encryption</span></span>
<span data-ttu-id="128c1-116">En översikt av klientsidan kryptering för Azure Storage finns [kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="128c1-116">For an overview of client-side encryption for Azure Storage, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)</span></span>

<span data-ttu-id="128c1-117">Här är en kort beskrivning av hur kryptering på serversidan för klienten fungerar:</span><span class="sxs-lookup"><span data-stu-id="128c1-117">Here is a brief description of how client side encryption works:</span></span>

1. <span data-ttu-id="128c1-118">hello Azure Storage client SDK genererar en innehåll krypteringsnyckel (CEK), vilket är en gång Använd symmetrisk nyckel.</span><span class="sxs-lookup"><span data-stu-id="128c1-118">hello Azure Storage client SDK generates a content encryption key (CEK), which is a one-time-use symmetric key.</span></span>
2. <span data-ttu-id="128c1-119">Kundinformation krypteras med den här CEK.</span><span class="sxs-lookup"><span data-stu-id="128c1-119">Customer data is encrypted using this CEK.</span></span>
3. <span data-ttu-id="128c1-120">Hej kapslas CEK sedan (krypterade) med hjälp av krypteringsnyckeln för hello nyckel (KEK).</span><span class="sxs-lookup"><span data-stu-id="128c1-120">hello CEK is then wrapped (encrypted) using hello key encryption key (KEK).</span></span> <span data-ttu-id="128c1-121">Hej KEK identifieras med en nyckelidentifierare och kan vara ett asymmetriskt nyckelpar eller en symmetrisk nyckel och kan hanteras lokalt eller lagras i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="128c1-121">hello KEK is identified by a key identifier and can be an asymmetric key pair or a symmetric key and can be managed locally or stored in Azure Key Vault.</span></span> <span data-ttu-id="128c1-122">hello lagring själva klienten har aldrig åtkomst toohello KEK.</span><span class="sxs-lookup"><span data-stu-id="128c1-122">hello Storage client itself never has access toohello KEK.</span></span> <span data-ttu-id="128c1-123">Den anropar bara hello viktiga radbrytning algoritmen som tillhandahålls av Key Vault.</span><span class="sxs-lookup"><span data-stu-id="128c1-123">It just invokes hello key wrapping algorithm that is provided by Key Vault.</span></span> <span data-ttu-id="128c1-124">Kunder kan välja toouse anpassade providers för nyckeln radbrytning/uppackning om de vill.</span><span class="sxs-lookup"><span data-stu-id="128c1-124">Customers can choose toouse custom providers for key wrapping/unwrapping if they want.</span></span>
4. <span data-ttu-id="128c1-125">hello krypterade data har sedan överföra toohello Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="128c1-125">hello encrypted data is then uploaded toohello Azure Storage service.</span></span>

## <a name="set-up-your-azure-key-vault"></a><span data-ttu-id="128c1-126">Konfigurera Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="128c1-126">Set up your Azure Key Vault</span></span>
<span data-ttu-id="128c1-127">I ordning tooproceed med den här självstudiekursen, behöver du toodo hello följa stegen som beskrivs i hello självstudiekursen [Kom igång med Azure Key Vault](../../key-vault/key-vault-get-started.md):</span><span class="sxs-lookup"><span data-stu-id="128c1-127">In order tooproceed with this tutorial, you need toodo hello following steps, which are outlined in hello tutorial  [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md):</span></span>

* <span data-ttu-id="128c1-128">Skapa en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="128c1-128">Create a key vault.</span></span>
* <span data-ttu-id="128c1-129">Lägg till en nyckel eller Hemlig toohello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="128c1-129">Add a key or secret toohello key vault.</span></span>
* <span data-ttu-id="128c1-130">Registrera ett program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="128c1-130">Register an application with Azure Active Directory.</span></span>
* <span data-ttu-id="128c1-131">Auktorisera hello programmet toouse hello nyckel eller hemlighet.</span><span class="sxs-lookup"><span data-stu-id="128c1-131">Authorize hello application toouse hello key or secret.</span></span>

<span data-ttu-id="128c1-132">Se anteckna hello ClientID och ClientSecret som genererats när du registrerar ett program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="128c1-132">Make note of hello ClientID and ClientSecret that were generated when registering an application with Azure Active Directory.</span></span>

<span data-ttu-id="128c1-133">Skapa båda nycklarna i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="128c1-133">Create both keys in hello key vault.</span></span> <span data-ttu-id="128c1-134">Vi förutsätter hello resten av hello kursen att du har använt hello följande namn: ContosoKeyVault och TestRSAKey1.</span><span class="sxs-lookup"><span data-stu-id="128c1-134">We assume for hello rest of hello tutorial that you have used hello following names: ContosoKeyVault and TestRSAKey1.</span></span>

## <a name="create-a-console-application-with-packages-and-appsettings"></a><span data-ttu-id="128c1-135">Skapa ett konsolprogram med paket och AppSettings</span><span class="sxs-lookup"><span data-stu-id="128c1-135">Create a console application with packages and AppSettings</span></span>
<span data-ttu-id="128c1-136">Skapa ett nytt konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="128c1-136">In Visual Studio, create a new console application.</span></span>

<span data-ttu-id="128c1-137">Lägg till nödvändiga nuget-paket i hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="128c1-137">Add necessary nuget packages in hello Package Manager Console.</span></span>

```
Install-Package WindowsAzure.Storage

// This is hello latest stable release for ADAL.
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

<span data-ttu-id="128c1-138">Lägg till AppSettings toohello App.Config.</span><span class="sxs-lookup"><span data-stu-id="128c1-138">Add AppSettings toohello App.Config.</span></span>

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

<span data-ttu-id="128c1-139">Lägg till följande hello `using` -satser och se till att tooadd referens tooSystem.Configuration toohello projektet.</span><span class="sxs-lookup"><span data-stu-id="128c1-139">Add hello following `using` statements and make sure tooadd a reference tooSystem.Configuration toohello project.</span></span>

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

## <a name="add-a-method-tooget-a-token-tooyour-console-application"></a><span data-ttu-id="128c1-140">Lägg till en metod tooget ett token tooyour konsolprogram</span><span class="sxs-lookup"><span data-stu-id="128c1-140">Add a method tooget a token tooyour console application</span></span>
<span data-ttu-id="128c1-141">hello används följande metod av Key Vault-klasser som behöver tooauthenticate för åtkomst tooyour nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="128c1-141">hello following method is used by Key Vault classes that need tooauthenticate for access tooyour key vault.</span></span>

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

## <a name="access-storage-and-key-vault-in-your-program"></a><span data-ttu-id="128c1-142">Komma åt lagringsutrymme och Key Vault i ditt program</span><span class="sxs-lookup"><span data-stu-id="128c1-142">Access Storage and Key Vault in your program</span></span>
<span data-ttu-id="128c1-143">Lägg till följande kod hello i hello huvudfunktionen.</span><span class="sxs-lookup"><span data-stu-id="128c1-143">In hello Main function, add hello following code.</span></span>

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
> <span data-ttu-id="128c1-144">Key Vault Nätverksobjektmodellerna</span><span class="sxs-lookup"><span data-stu-id="128c1-144">Key Vault Object Models</span></span>
> 
> <span data-ttu-id="128c1-145">Det är viktigt toounderstand att det finns faktiskt två Key Vault objektet modeller toobe medveten om: en är baserad på hello REST-API (KeyVault namnområde) och hello andra är ett tillägg för kryptering på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="128c1-145">It is important toounderstand that there are actually two Key Vault object models toobe aware of: one is based on hello REST API (KeyVault namespace) and hello other is an extension for client-side encryption.</span></span>
> 
> <span data-ttu-id="128c1-146">hello Key Vault klienten samverkar med hello REST-API och förstår JSON Web nycklar och hemligheter för hello två typer av saker som ingår i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="128c1-146">hello Key Vault Client interacts with hello REST API and understands JSON Web Keys and secrets for hello two kinds of things that are contained in Key Vault.</span></span>
> 
> <span data-ttu-id="128c1-147">hello Key Vault tillägg är klasser som verkar vara skapats specifikt för klientsidan kryptering i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="128c1-147">hello Key Vault Extensions are classes that seem specifically created for client-side encryption in Azure Storage.</span></span> <span data-ttu-id="128c1-148">De innehåller ett gränssnitt för nycklar (IKey) och klasser som baseras på hello konceptet för en nyckel-matchare.</span><span class="sxs-lookup"><span data-stu-id="128c1-148">They contain an interface for keys (IKey) and classes based on hello concept of a Key Resolver.</span></span> <span data-ttu-id="128c1-149">Det finns två implementeringar av du behöver tooknow IKey: RSAKey och SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="128c1-149">There are two implementations of IKey that you need tooknow: RSAKey and SymmetricKey.</span></span> <span data-ttu-id="128c1-150">Nu de råkar toocoincide med hello saker som ingår i ett Nyckelvalv, men då de är oberoende klasser (så inte implementerar IKey hello nyckel och Hemlig hämtas av hello Key Vault-klienten).</span><span class="sxs-lookup"><span data-stu-id="128c1-150">Now they happen toocoincide with hello things that are contained in a Key Vault, but at this point they are independent classes (so hello Key and Secret retrieved by hello Key Vault Client do not implement IKey).</span></span>
> 
> 

## <a name="encrypt-blob-and-upload"></a><span data-ttu-id="128c1-151">Kryptera blob och ladda upp</span><span class="sxs-lookup"><span data-stu-id="128c1-151">Encrypt blob and upload</span></span>
<span data-ttu-id="128c1-152">Lägg till följande hello code tooencrypt en blobb och överför den tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="128c1-152">Add hello following code tooencrypt a blob and upload it tooyour Azure storage account.</span></span> <span data-ttu-id="128c1-153">Hej **ResolveKeyAsync** returnerar metoden som används för en IKey.</span><span class="sxs-lookup"><span data-stu-id="128c1-153">hello **ResolveKeyAsync** method that is used returns an IKey.</span></span>

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

<span data-ttu-id="128c1-154">Följande är en skärmbild från hello [klassiska Azure-portalen](https://manage.windowsazure.com) för en blob som har krypterats med hjälp av klientsidan kryptering med en nyckel som lagras i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="128c1-154">Following is a screenshot from hello [Azure Classic Portal](https://manage.windowsazure.com) for a blob that has been encrypted by using client-side encryption with a key stored in Key Vault.</span></span> <span data-ttu-id="128c1-155">Hej **KeyId** egenskapen är hello URI för hello nyckeln i Nyckelvalvet som fungerar som hello KEK.</span><span class="sxs-lookup"><span data-stu-id="128c1-155">hello **KeyId** property is hello URI for hello key in Key Vault that acts as hello KEK.</span></span> <span data-ttu-id="128c1-156">Hej **EncryptedKey** egenskap innehåller hello krypterad version av hello CEK.</span><span class="sxs-lookup"><span data-stu-id="128c1-156">hello **EncryptedKey** property contains hello encrypted version of hello CEK.</span></span>

![Skärmbild som visar Blob-metadata som innehåller krypteringsmetadata](./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png)

> [!NOTE]
> <span data-ttu-id="128c1-158">Om du tittar på hello BlobEncryptionPolicy konstruktorn visas att den kan acceptera en nyckel och/eller en DNS-matchare.</span><span class="sxs-lookup"><span data-stu-id="128c1-158">If you look at hello BlobEncryptionPolicy constructor, you will see that it can accept a key and/or a resolver.</span></span> <span data-ttu-id="128c1-159">Tänk som nu har du inte använda en DNS-matchare för kryptering eftersom den har för närvarande inte stöder en standard-nyckel.</span><span class="sxs-lookup"><span data-stu-id="128c1-159">Be aware that right now you cannot use a resolver for encryption because it does not currently support a default key.</span></span>
> 
> 

## <a name="decrypt-blob-and-download"></a><span data-ttu-id="128c1-160">Dekryptera blob och ladda ned</span><span class="sxs-lookup"><span data-stu-id="128c1-160">Decrypt blob and download</span></span>
<span data-ttu-id="128c1-161">Dekryptering är verkligen när med hello matcharen vits med klasser.</span><span class="sxs-lookup"><span data-stu-id="128c1-161">Decryption is really when using hello Resolver classes make sense.</span></span> <span data-ttu-id="128c1-162">hello-ID för hello-nyckel som används för kryptering är associerad med hello blob i dess metadata, så det finns ingen anledning att du tooretrieve hello nyckel och Kom ihåg hello kopplingen mellan nyckel och blob.</span><span class="sxs-lookup"><span data-stu-id="128c1-162">hello ID of hello key used for encryption is associated with hello blob in its metadata, so there is no reason for you tooretrieve hello key and remember hello association between key and blob.</span></span> <span data-ttu-id="128c1-163">Du har precis toomake att hello nyckeln finns kvar i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="128c1-163">You just have toomake sure that hello key remains in Key Vault.</span></span>   

<span data-ttu-id="128c1-164">hello privata nyckeln för en RSA-nyckel finns kvar i Nyckelvalvet, så för dekryptering toooccur hello krypterade nyckeln från hello blobbmetadata som innehåller hello CEK skickas tooKey valvet för dekryptering.</span><span class="sxs-lookup"><span data-stu-id="128c1-164">hello private key of an RSA Key remains in Key Vault, so for decryption toooccur, hello Encrypted Key from hello blob metadata that contains hello CEK is sent tooKey Vault for decryption.</span></span>

<span data-ttu-id="128c1-165">Lägg till följande toodecrypt hello blob som du just har överfört hello.</span><span class="sxs-lookup"><span data-stu-id="128c1-165">Add hello following toodecrypt hello blob that you just uploaded.</span></span>

```csharp
// In this case, we will not pass a key and only pass hello resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> <span data-ttu-id="128c1-166">Det finns några andra typer av matchare toomake nyckelhantering enklare, inklusive: AggregateKeyResolver och CachingKeyResolver.</span><span class="sxs-lookup"><span data-stu-id="128c1-166">There are a couple of other kinds of resolvers toomake key management easier, including: AggregateKeyResolver and CachingKeyResolver.</span></span>
> 
> 

## <a name="use-key-vault-secrets"></a><span data-ttu-id="128c1-167">Använd Key Vault hemligheter</span><span class="sxs-lookup"><span data-stu-id="128c1-167">Use Key Vault secrets</span></span>
<span data-ttu-id="128c1-168">hello sätt toouse en hemlighet med kryptering på klientsidan är via hello SymmetricKey klassen eftersom en hemlighet är i grunden en symmetrisk nyckel.</span><span class="sxs-lookup"><span data-stu-id="128c1-168">hello way toouse a secret with client-side encryption is via hello SymmetricKey class because a secret is essentially a symmetric key.</span></span> <span data-ttu-id="128c1-169">Men som nämns ovan, mappar en hemlighet i Nyckelvalvet inte exakt tooa SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="128c1-169">But, as noted above, a secret in Key Vault does not map exactly tooa SymmetricKey.</span></span> <span data-ttu-id="128c1-170">Det finns några saker toounderstand:</span><span class="sxs-lookup"><span data-stu-id="128c1-170">There are a few things toounderstand:</span></span>

* <span data-ttu-id="128c1-171">hello nyckeln i en SymmetricKey har toobe en fast längd: 128, 192, 256, 384 eller 512 bitar.</span><span class="sxs-lookup"><span data-stu-id="128c1-171">hello key in a SymmetricKey has toobe a fixed length: 128, 192, 256, 384, or 512 bits.</span></span>
* <span data-ttu-id="128c1-172">hello nyckeln i en SymmetricKey måste vara Base64-kodad.</span><span class="sxs-lookup"><span data-stu-id="128c1-172">hello key in a SymmetricKey should be Base64 encoded.</span></span>
* <span data-ttu-id="128c1-173">En hemlighet i Nyckelvalvet som ska användas som en SymmetricKey måste toohave innehållstypen för ”program/oktettsröm” i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="128c1-173">A Key Vault secret that will be used as a SymmetricKey needs toohave a Content Type of "application/octet-stream" in Key Vault.</span></span>

<span data-ttu-id="128c1-174">Här är ett exempel i PowerShell för att skapa en hemlighet i Nyckelvalvet som kan användas som en SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="128c1-174">Here is an example in PowerShell of creating a secret in Key Vault that can be used as a SymmetricKey.</span></span>
<span data-ttu-id="128c1-175">Observera att hello hårda kodat värde, $key, för demonstration ändamål endast.</span><span class="sxs-lookup"><span data-stu-id="128c1-175">Please note that hello hard coded value, $key, is for demonstration purpose only.</span></span> <span data-ttu-id="128c1-176">I din kod ska du toogenerate den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="128c1-176">In your own code you'll want toogenerate this key.</span></span>

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

<span data-ttu-id="128c1-177">Du kan använda samma anropa som före tooretrieve detta hemliga som en SymmetricKey hello i din konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="128c1-177">In your console application, you can use hello same call as before tooretrieve this secret as a SymmetricKey.</span></span>

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
<span data-ttu-id="128c1-178">Det var allt.</span><span class="sxs-lookup"><span data-stu-id="128c1-178">That's it.</span></span> <span data-ttu-id="128c1-179">Ha det så kul!</span><span class="sxs-lookup"><span data-stu-id="128c1-179">Enjoy!</span></span>

## <a name="next-steps"></a><span data-ttu-id="128c1-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="128c1-180">Next steps</span></span>
<span data-ttu-id="128c1-181">Mer information om hur du använder Microsoft Azure Storage med C# finns [Microsoft Azure Storage-klientbibliotek för .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="128c1-181">For more information about using Microsoft Azure Storage with C#, see [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="128c1-182">Mer information om hello Blob REST-API finns [REST-API för Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd135733.aspx).</span><span class="sxs-lookup"><span data-stu-id="128c1-182">For more information about hello Blob REST API, see [Blob Service REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).</span></span>

<span data-ttu-id="128c1-183">Hello senaste informationen om Microsoft Azure Storage, gå toohello [Microsoft Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/).</span><span class="sxs-lookup"><span data-stu-id="128c1-183">For hello latest information on Microsoft Azure Storage, go toohello [Microsoft Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/).</span></span>
