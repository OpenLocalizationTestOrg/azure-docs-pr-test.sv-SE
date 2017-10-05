---
title: "Självstudier: Kryptera och dekryptera blobbar i Azure Storage med hjälp av Azure Key Vault | Microsoft Docs"
description: "Hur du krypterar och dekrypterar en blob med kryptering på klientsidan för Microsoft Azure Storage med Azure Key Vault."
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
ms.openlocfilehash: a2a3a4773d33fe6b8589ad8d9d219acda4d1015e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a><span data-ttu-id="838ef-103">Självstudier: Kryptera och dekryptera blobbar i Microsoft Azure Storage med hjälp av Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="838ef-103">Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault</span></span>
## <a name="introduction"></a><span data-ttu-id="838ef-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="838ef-104">Introduction</span></span>
<span data-ttu-id="838ef-105">Den här självstudiekursen beskrivs hur du gör användning av klientsidan lagringskryptering med Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="838ef-105">This tutorial covers how to make use of client-side storage encryption with Azure Key Vault.</span></span> <span data-ttu-id="838ef-106">Den vägleder dig igenom hur du krypterar och dekrypterar en blobb i ett konsolprogram med hjälp av dessa tekniker.</span><span class="sxs-lookup"><span data-stu-id="838ef-106">It walks you through how to encrypt and decrypt a blob in a console application using these technologies.</span></span>

<span data-ttu-id="838ef-107">**Uppskattad tidsåtgång:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="838ef-107">**Estimated time to complete:** 20 minutes</span></span>

<span data-ttu-id="838ef-108">Mer information om Azure Key Vault finns [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="838ef-108">For overview information about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md).</span></span>

<span data-ttu-id="838ef-109">Mer information om klientens kryptering för Azure Storage finns [kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="838ef-109">For overview information about client-side encryption for Azure Storage, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="838ef-110">Krav</span><span class="sxs-lookup"><span data-stu-id="838ef-110">Prerequisites</span></span>
<span data-ttu-id="838ef-111">För att kunna slutföra den här självstudiekursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="838ef-111">To complete this tutorial, you must have the following:</span></span>

* <span data-ttu-id="838ef-112">Ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="838ef-112">An Azure Storage account</span></span>
* <span data-ttu-id="838ef-113">Visual Studio 2013 eller senare</span><span class="sxs-lookup"><span data-stu-id="838ef-113">Visual Studio 2013 or later</span></span>
* <span data-ttu-id="838ef-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="838ef-114">Azure PowerShell</span></span>

## <a name="overview-of-client-side-encryption"></a><span data-ttu-id="838ef-115">Översikt över kryptering på klientsidan</span><span class="sxs-lookup"><span data-stu-id="838ef-115">Overview of client-side encryption</span></span>
<span data-ttu-id="838ef-116">En översikt av klientsidan kryptering för Azure Storage finns [kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="838ef-116">For an overview of client-side encryption for Azure Storage, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)</span></span>

<span data-ttu-id="838ef-117">Här är en kort beskrivning av hur kryptering på serversidan för klienten fungerar:</span><span class="sxs-lookup"><span data-stu-id="838ef-117">Here is a brief description of how client side encryption works:</span></span>

1. <span data-ttu-id="838ef-118">Azure Storage client SDK genererar en innehåll krypteringsnyckel (CEK), vilket är en gång Använd symmetrisk nyckel.</span><span class="sxs-lookup"><span data-stu-id="838ef-118">The Azure Storage client SDK generates a content encryption key (CEK), which is a one-time-use symmetric key.</span></span>
2. <span data-ttu-id="838ef-119">Kundinformation krypteras med den här CEK.</span><span class="sxs-lookup"><span data-stu-id="838ef-119">Customer data is encrypted using this CEK.</span></span>
3. <span data-ttu-id="838ef-120">CEK kapslas sedan (krypterade) med hjälp av krypteringsnyckel (KEK).</span><span class="sxs-lookup"><span data-stu-id="838ef-120">The CEK is then wrapped (encrypted) using the key encryption key (KEK).</span></span> <span data-ttu-id="838ef-121">KEK kan identifieras med en nyckelidentifierare och vara ett asymmetriskt nyckelpar eller en symmetrisk nyckel och kan hanteras lokalt eller lagras i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="838ef-121">The KEK is identified by a key identifier and can be an asymmetric key pair or a symmetric key and can be managed locally or stored in Azure Key Vault.</span></span> <span data-ttu-id="838ef-122">Klientens lagring har aldrig åtkomst till KEK.</span><span class="sxs-lookup"><span data-stu-id="838ef-122">The Storage client itself never has access to the KEK.</span></span> <span data-ttu-id="838ef-123">Den anropar bara viktiga radbrytning algoritmen som tillhandahålls av Key Vault.</span><span class="sxs-lookup"><span data-stu-id="838ef-123">It just invokes the key wrapping algorithm that is provided by Key Vault.</span></span> <span data-ttu-id="838ef-124">Kunder kan välja att använda anpassade providers för för radbrytning/uppackning om de vill.</span><span class="sxs-lookup"><span data-stu-id="838ef-124">Customers can choose to use custom providers for key wrapping/unwrapping if they want.</span></span>
4. <span data-ttu-id="838ef-125">Krypterade data överförs sedan till Azure Storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="838ef-125">The encrypted data is then uploaded to the Azure Storage service.</span></span>

## <a name="set-up-your-azure-key-vault"></a><span data-ttu-id="838ef-126">Konfigurera Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="838ef-126">Set up your Azure Key Vault</span></span>
<span data-ttu-id="838ef-127">För att fortsätta med den här kursen behöver du följande steg, som beskrivs i kursen [Kom igång med Azure Key Vault](../../key-vault/key-vault-get-started.md):</span><span class="sxs-lookup"><span data-stu-id="838ef-127">In order to proceed with this tutorial, you need to do the following steps, which are outlined in the tutorial  [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md):</span></span>

* <span data-ttu-id="838ef-128">Skapa en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="838ef-128">Create a key vault.</span></span>
* <span data-ttu-id="838ef-129">Lägga till en nyckel eller hemlighet i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="838ef-129">Add a key or secret to the key vault.</span></span>
* <span data-ttu-id="838ef-130">Registrera ett program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="838ef-130">Register an application with Azure Active Directory.</span></span>
* <span data-ttu-id="838ef-131">Tillåta program att använda nyckel eller hemlighet.</span><span class="sxs-lookup"><span data-stu-id="838ef-131">Authorize the application to use the key or secret.</span></span>

<span data-ttu-id="838ef-132">Anteckna ClientID och ClientSecret som genererats när du registrerar ett program med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="838ef-132">Make note of the ClientID and ClientSecret that were generated when registering an application with Azure Active Directory.</span></span>

<span data-ttu-id="838ef-133">Skapa båda nycklarna i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="838ef-133">Create both keys in the key vault.</span></span> <span data-ttu-id="838ef-134">Vi förutsätter för resten av guiden att du har använt följande namn: ContosoKeyVault och TestRSAKey1.</span><span class="sxs-lookup"><span data-stu-id="838ef-134">We assume for the rest of the tutorial that you have used the following names: ContosoKeyVault and TestRSAKey1.</span></span>

## <a name="create-a-console-application-with-packages-and-appsettings"></a><span data-ttu-id="838ef-135">Skapa ett konsolprogram med paket och AppSettings</span><span class="sxs-lookup"><span data-stu-id="838ef-135">Create a console application with packages and AppSettings</span></span>
<span data-ttu-id="838ef-136">Skapa ett nytt konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="838ef-136">In Visual Studio, create a new console application.</span></span>

<span data-ttu-id="838ef-137">Lägg till nödvändiga nuget-paket i Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="838ef-137">Add necessary nuget packages in the Package Manager Console.</span></span>

```
Install-Package WindowsAzure.Storage

// This is the latest stable release for ADAL.
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

<span data-ttu-id="838ef-138">Lägg till AppSettings i App.Config.</span><span class="sxs-lookup"><span data-stu-id="838ef-138">Add AppSettings to the App.Config.</span></span>

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

<span data-ttu-id="838ef-139">Lägg till följande `using` -satser och se till att lägga till en referens till System.Configuration i projektet.</span><span class="sxs-lookup"><span data-stu-id="838ef-139">Add the following `using` statements and make sure to add a reference to System.Configuration to the project.</span></span>

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

## <a name="add-a-method-to-get-a-token-to-your-console-application"></a><span data-ttu-id="838ef-140">Lägg till en metod för att hämta en token i konsoltillämpning</span><span class="sxs-lookup"><span data-stu-id="838ef-140">Add a method to get a token to your console application</span></span>
<span data-ttu-id="838ef-141">Följande metod används av Key Vault-klasser som måste autentiseras för åtkomst till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="838ef-141">The following method is used by Key Vault classes that need to authenticate for access to your key vault.</span></span>

```csharp
private async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(
        ConfigurationManager.AppSettings["clientId"],
        ConfigurationManager.AppSettings["clientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

## <a name="access-storage-and-key-vault-in-your-program"></a><span data-ttu-id="838ef-142">Komma åt lagringsutrymme och Key Vault i ditt program</span><span class="sxs-lookup"><span data-stu-id="838ef-142">Access Storage and Key Vault in your program</span></span>
<span data-ttu-id="838ef-143">Lägg till följande kod i funktionen Main.</span><span class="sxs-lookup"><span data-stu-id="838ef-143">In the Main function, add the following code.</span></span>

```csharp
// This is standard code to interact with Blob storage.
StorageCredentials creds = new StorageCredentials(
    ConfigurationManager.AppSettings["accountName"],
       ConfigurationManager.AppSettings["accountKey"]);
CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
CloudBlobClient client = account.CreateCloudBlobClient();
CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
contain.CreateIfNotExists();

// The Resolver object is used to interact with Key Vault for Azure Storage.
// This is where the GetToken method from above is used.
KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);
```

> [!NOTE]
> <span data-ttu-id="838ef-144">Key Vault Nätverksobjektmodellerna</span><span class="sxs-lookup"><span data-stu-id="838ef-144">Key Vault Object Models</span></span>
> 
> <span data-ttu-id="838ef-145">Det är viktigt att förstå att det finns faktiskt två Key Vault nätverksobjektmodellerna vara medveten om: en baseras på REST-API (KeyVault namnområde) och den andra är ett tillägg för kryptering på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="838ef-145">It is important to understand that there are actually two Key Vault object models to be aware of: one is based on the REST API (KeyVault namespace) and the other is an extension for client-side encryption.</span></span>
> 
> <span data-ttu-id="838ef-146">Key Vault klienten samverkar med REST-API och förstår JSON Web nycklar och hemligheter för de två typerna av saker som ingår i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="838ef-146">The Key Vault Client interacts with the REST API and understands JSON Web Keys and secrets for the two kinds of things that are contained in Key Vault.</span></span>
> 
> <span data-ttu-id="838ef-147">Key Vault-tillägg är klasser som verkar vara skapats specifikt för klientsidan kryptering i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="838ef-147">The Key Vault Extensions are classes that seem specifically created for client-side encryption in Azure Storage.</span></span> <span data-ttu-id="838ef-148">De innehåller ett gränssnitt för nycklar (IKey) och klasser som baseras på konceptet för en nyckel-matchare.</span><span class="sxs-lookup"><span data-stu-id="838ef-148">They contain an interface for keys (IKey) and classes based on the concept of a Key Resolver.</span></span> <span data-ttu-id="838ef-149">Det finns två implementeringar av IKey som du behöver veta: RSAKey och SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="838ef-149">There are two implementations of IKey that you need to know: RSAKey and SymmetricKey.</span></span> <span data-ttu-id="838ef-150">Nu de råkar sammanfaller med vad som ingår i ett Nyckelvalv, men då de är oberoende klasser (så IKey inte implementerar en nyckel och Hemlig hämtas av Key Vault klienten).</span><span class="sxs-lookup"><span data-stu-id="838ef-150">Now they happen to coincide with the things that are contained in a Key Vault, but at this point they are independent classes (so the Key and Secret retrieved by the Key Vault Client do not implement IKey).</span></span>
> 
> 

## <a name="encrypt-blob-and-upload"></a><span data-ttu-id="838ef-151">Kryptera blob och ladda upp</span><span class="sxs-lookup"><span data-stu-id="838ef-151">Encrypt blob and upload</span></span>
<span data-ttu-id="838ef-152">Lägg till följande kod för att kryptera en blob och överföra den till Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="838ef-152">Add the following code to encrypt a blob and upload it to your Azure storage account.</span></span> <span data-ttu-id="838ef-153">Den **ResolveKeyAsync** returnerar metoden som används för en IKey.</span><span class="sxs-lookup"><span data-stu-id="838ef-153">The **ResolveKeyAsync** method that is used returns an IKey.</span></span>

```csharp
// Retrieve the key that you created previously.
// The IKey that is returned here is an RsaKey.
// Remember that we used the names contosokeyvault and testrsakey1.
var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();

// Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

// Reference a block blob.
CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

// Upload using the UploadFromStream method.
using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
    blob.UploadFromStream(stream, stream.Length, null, options, null);
```

<span data-ttu-id="838ef-154">Följande är en skärmbild från den [klassiska Azure-portalen](https://manage.windowsazure.com) för en blob som har krypterats med hjälp av klientsidan kryptering med en nyckel som lagras i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="838ef-154">Following is a screenshot from the [Azure Classic Portal](https://manage.windowsazure.com) for a blob that has been encrypted by using client-side encryption with a key stored in Key Vault.</span></span> <span data-ttu-id="838ef-155">Den **KeyId** egenskapen är URI: N för nyckeln i Nyckelvalvet som fungerar som KEK.</span><span class="sxs-lookup"><span data-stu-id="838ef-155">The **KeyId** property is the URI for the key in Key Vault that acts as the KEK.</span></span> <span data-ttu-id="838ef-156">Den **EncryptedKey** -egenskapen innehåller den krypterade versionen av CEK.</span><span class="sxs-lookup"><span data-stu-id="838ef-156">The **EncryptedKey** property contains the encrypted version of the CEK.</span></span>

![Skärmbild som visar Blob-metadata som innehåller krypteringsmetadata](./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png)

> [!NOTE]
> <span data-ttu-id="838ef-158">Om du tittar på konstruktorn BlobEncryptionPolicy visas att den kan acceptera en nyckel och/eller en DNS-matchare.</span><span class="sxs-lookup"><span data-stu-id="838ef-158">If you look at the BlobEncryptionPolicy constructor, you will see that it can accept a key and/or a resolver.</span></span> <span data-ttu-id="838ef-159">Tänk som nu har du inte använda en DNS-matchare för kryptering eftersom den har för närvarande inte stöder en standard-nyckel.</span><span class="sxs-lookup"><span data-stu-id="838ef-159">Be aware that right now you cannot use a resolver for encryption because it does not currently support a default key.</span></span>
> 
> 

## <a name="decrypt-blob-and-download"></a><span data-ttu-id="838ef-160">Dekryptera blob och ladda ned</span><span class="sxs-lookup"><span data-stu-id="838ef-160">Decrypt blob and download</span></span>
<span data-ttu-id="838ef-161">Dekryptering är verkligen när passar med DNS-matchare klasser.</span><span class="sxs-lookup"><span data-stu-id="838ef-161">Decryption is really when using the Resolver classes make sense.</span></span> <span data-ttu-id="838ef-162">ID för den nyckel som används för kryptering är associerad med blob i dess metadata, så det finns ingen anledning att hämta nyckeln och spara associationen mellan nyckel och blob.</span><span class="sxs-lookup"><span data-stu-id="838ef-162">The ID of the key used for encryption is associated with the blob in its metadata, so there is no reason for you to retrieve the key and remember the association between key and blob.</span></span> <span data-ttu-id="838ef-163">Du behöver bara kontrollera att nyckeln finns kvar i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="838ef-163">You just have to make sure that the key remains in Key Vault.</span></span>   

<span data-ttu-id="838ef-164">Den privata nyckeln för en RSA-nyckel som finns kvar i Nyckelvalvet, så för dekryptering ska ske, den krypterade nyckeln från blobmetadata som innehåller CEK skickas till Key Vault för dekryptering.</span><span class="sxs-lookup"><span data-stu-id="838ef-164">The private key of an RSA Key remains in Key Vault, so for decryption to occur, the Encrypted Key from the blob metadata that contains the CEK is sent to Key Vault for decryption.</span></span>

<span data-ttu-id="838ef-165">Lägg till följande för att dekryptera blob som du just har överfört.</span><span class="sxs-lookup"><span data-stu-id="838ef-165">Add the following to decrypt the blob that you just uploaded.</span></span>

```csharp
// In this case, we will not pass a key and only pass the resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> <span data-ttu-id="838ef-166">Det finns några andra typer av matchare att underlätta hantering av nycklar, inklusive: AggregateKeyResolver och CachingKeyResolver.</span><span class="sxs-lookup"><span data-stu-id="838ef-166">There are a couple of other kinds of resolvers to make key management easier, including: AggregateKeyResolver and CachingKeyResolver.</span></span>
> 
> 

## <a name="use-key-vault-secrets"></a><span data-ttu-id="838ef-167">Använd Key Vault hemligheter</span><span class="sxs-lookup"><span data-stu-id="838ef-167">Use Key Vault secrets</span></span>
<span data-ttu-id="838ef-168">Sätt att använda en hemlighet med kryptering på klientsidan är klassen SymmetricKey eftersom en hemlighet är i grunden en symmetrisk nyckel.</span><span class="sxs-lookup"><span data-stu-id="838ef-168">The way to use a secret with client-side encryption is via the SymmetricKey class because a secret is essentially a symmetric key.</span></span> <span data-ttu-id="838ef-169">Men som nämns ovan, en hemlighet i Nyckelvalvet mappar inte exakt med en SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="838ef-169">But, as noted above, a secret in Key Vault does not map exactly to a SymmetricKey.</span></span> <span data-ttu-id="838ef-170">Det finns några saker att förstå:</span><span class="sxs-lookup"><span data-stu-id="838ef-170">There are a few things to understand:</span></span>

* <span data-ttu-id="838ef-171">Nyckeln i en SymmetricKey måste vara en fast längd: 128, 192, 256, 384 eller 512 bitar.</span><span class="sxs-lookup"><span data-stu-id="838ef-171">The key in a SymmetricKey has to be a fixed length: 128, 192, 256, 384, or 512 bits.</span></span>
* <span data-ttu-id="838ef-172">Nyckeln i en SymmetricKey måste vara Base64-kodad.</span><span class="sxs-lookup"><span data-stu-id="838ef-172">The key in a SymmetricKey should be Base64 encoded.</span></span>
* <span data-ttu-id="838ef-173">En hemlighet i Nyckelvalvet som ska användas som en SymmetricKey måste ha en Content-Type för ”program/oktett-ström” i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="838ef-173">A Key Vault secret that will be used as a SymmetricKey needs to have a Content Type of "application/octet-stream" in Key Vault.</span></span>

<span data-ttu-id="838ef-174">Här är ett exempel i PowerShell för att skapa en hemlighet i Nyckelvalvet som kan användas som en SymmetricKey.</span><span class="sxs-lookup"><span data-stu-id="838ef-174">Here is an example in PowerShell of creating a secret in Key Vault that can be used as a SymmetricKey.</span></span>
<span data-ttu-id="838ef-175">Observera att värdet hårdkodade $key, för demonstration ändamål endast.</span><span class="sxs-lookup"><span data-stu-id="838ef-175">Please note that the hard coded value, $key, is for demonstration purpose only.</span></span> <span data-ttu-id="838ef-176">I din kod vill du skapa den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="838ef-176">In your own code you'll want to generate this key.</span></span>

```csharp
// Here we are making a 128-bit key so we have 16 characters.
//     The characters are in the ASCII range of UTF8 so they are
//    each 1 byte. 16 x 8 = 128.
$key = "qwertyuiopasdfgh"
$b = [System.Text.Encoding]::UTF8.GetBytes($key)
$enc = [System.Convert]::ToBase64String($b)
$secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

// Substitute the VaultName and Name in this command.
$secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"
```

<span data-ttu-id="838ef-177">Du kan använda samma anropet som innan att hämta den här hemligheten som en SymmetricKey i din konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="838ef-177">In your console application, you can use the same call as before to retrieve this secret as a SymmetricKey.</span></span>

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
<span data-ttu-id="838ef-178">Det var allt.</span><span class="sxs-lookup"><span data-stu-id="838ef-178">That's it.</span></span> <span data-ttu-id="838ef-179">Ha det så kul!</span><span class="sxs-lookup"><span data-stu-id="838ef-179">Enjoy!</span></span>

## <a name="next-steps"></a><span data-ttu-id="838ef-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="838ef-180">Next steps</span></span>
<span data-ttu-id="838ef-181">Mer information om hur du använder Microsoft Azure Storage med C# finns [Microsoft Azure Storage-klientbibliotek för .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="838ef-181">For more information about using Microsoft Azure Storage with C#, see [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="838ef-182">Mer information om Blob REST-API finns [REST-API för Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd135733.aspx).</span><span class="sxs-lookup"><span data-stu-id="838ef-182">For more information about the Blob REST API, see [Blob Service REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).</span></span>

<span data-ttu-id="838ef-183">Den senaste informationen om Microsoft Azure Storage, gå till den [Microsoft Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/).</span><span class="sxs-lookup"><span data-stu-id="838ef-183">For the latest information on Microsoft Azure Storage, go to the [Microsoft Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/).</span></span>
