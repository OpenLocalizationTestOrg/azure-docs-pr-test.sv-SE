---
title: "Skapa och använda en signatur för delad åtkomst (SAS) med Azure Blob storage | Microsoft Docs"
description: "Den här kursen visar hur du skapar signaturer för delad åtkomst för användning med Blob storage och använda dem i ditt klientprogram."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 491e0b3c-76d4-4149-9a80-bbbd683b1f3e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/15/2017
ms.author: marsma
ms.openlocfilehash: ba78dd2bbcc68ffffeba59b1623891126baf656f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a><span data-ttu-id="846f3-103">Signaturer för delad åtkomst, del 2: Skapa och använda en SAS med Blob storage</span><span class="sxs-lookup"><span data-stu-id="846f3-103">Shared Access Signatures, Part 2: Create and use a SAS with Blob storage</span></span>

<span data-ttu-id="846f3-104">[Del 1](storage-dotnet-shared-access-signature-part-1.md) i den här kursen utforskade signaturer för delad åtkomst (SAS) och beskrivs bästa praxis för att använda dem.</span><span class="sxs-lookup"><span data-stu-id="846f3-104">[Part 1](storage-dotnet-shared-access-signature-part-1.md) of this tutorial explored shared access signatures (SAS) and explained best practices for using them.</span></span> <span data-ttu-id="846f3-105">Del 2 visar hur du skapar och sedan använda signaturer för delad åtkomst med Blob storage.</span><span class="sxs-lookup"><span data-stu-id="846f3-105">Part 2 shows you how to generate and then use shared access signatures with Blob storage.</span></span> <span data-ttu-id="846f3-106">Exemplen är skrivna i C# och använder Azure Storage-klientbiblioteket för .NET.</span><span class="sxs-lookup"><span data-stu-id="846f3-106">The examples are written in C# and use the Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="846f3-107">Exemplen i den här kursen:</span><span class="sxs-lookup"><span data-stu-id="846f3-107">The examples in this tutorial:</span></span>

* <span data-ttu-id="846f3-108">Generera en signatur för delad åtkomst i en behållare</span><span class="sxs-lookup"><span data-stu-id="846f3-108">Generate a shared access signature on a container</span></span>
* <span data-ttu-id="846f3-109">Generera en signatur för delad åtkomst på en blob</span><span class="sxs-lookup"><span data-stu-id="846f3-109">Generate a shared access signature on a blob</span></span>
* <span data-ttu-id="846f3-110">Skapa en princip för lagrade åtkomst för att hantera signaturer på en behållare resurser</span><span class="sxs-lookup"><span data-stu-id="846f3-110">Create a stored access policy to manage signatures on a container's resources</span></span>
* <span data-ttu-id="846f3-111">Testa signaturer för delad åtkomst i klientprogram</span><span class="sxs-lookup"><span data-stu-id="846f3-111">Test the shared access signatures in a client application</span></span>

## <a name="about-this-tutorial"></a><span data-ttu-id="846f3-112">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="846f3-112">About this tutorial</span></span>
<span data-ttu-id="846f3-113">I den här självstudiekursen skapar vi två konsolprogram som visar hur du skapar och använder signaturer för delad åtkomst för behållare och blobbar:</span><span class="sxs-lookup"><span data-stu-id="846f3-113">In this tutorial, we create two console applications that demonstrate creating and using shared access signatures for containers and blobs:</span></span>

<span data-ttu-id="846f3-114">**Programmet 1**: av hanteringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="846f3-114">**Application 1**: The management application.</span></span> <span data-ttu-id="846f3-115">Genererar en signatur för delad åtkomst för en behållare och en blob.</span><span class="sxs-lookup"><span data-stu-id="846f3-115">Generates a shared access signature for a container and a blob.</span></span> <span data-ttu-id="846f3-116">Innehåller åtkomstnyckeln för lagringskontot i källkoden.</span><span class="sxs-lookup"><span data-stu-id="846f3-116">Includes the storage account access key in source code.</span></span>

<span data-ttu-id="846f3-117">**Programmet 2**: klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="846f3-117">**Application 2**: The client application.</span></span> <span data-ttu-id="846f3-118">Ansluter till behållare och blob resurser med signaturer för delad åtkomst som skapats med det första programmet.</span><span class="sxs-lookup"><span data-stu-id="846f3-118">Accesses container and blob resources using the shared access signatures created with the first application.</span></span> <span data-ttu-id="846f3-119">Använder bara signaturerna för delad åtkomst till behållaren för åtkomst och resurser för blob - sker *inte* inkluderar åtkomstnyckeln för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="846f3-119">Uses only the shared access signatures to access container and blob resources--it does *not* include the storage account access key.</span></span>

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a><span data-ttu-id="846f3-120">Del 1: Skapa ett konsolprogram att generera signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="846f3-120">Part 1: Create a console application to generate shared access signatures</span></span>
<span data-ttu-id="846f3-121">Kontrollera först att du har Azure Storage-klientbiblioteket för .NET installerats.</span><span class="sxs-lookup"><span data-stu-id="846f3-121">First, ensure that you have the Azure Storage Client Library for .NET installed.</span></span> <span data-ttu-id="846f3-122">Du kan installera den [NuGet-paketet](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet-paketet") som innehåller den senaste sammansättningarna för klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="846f3-122">You can install the [NuGet package](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet package") containing the most up-to-date assemblies for the client library.</span></span> <span data-ttu-id="846f3-123">Det här är den rekommenderade metoden för att säkerställa att du har de senaste korrigeringarna.</span><span class="sxs-lookup"><span data-stu-id="846f3-123">This is the recommended method for ensuring that you have the most recent fixes.</span></span> <span data-ttu-id="846f3-124">Du kan också hämta klientbiblioteket som en del av den senaste versionen av den [Azure SDK för .NET](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="846f3-124">You can also download the client library as part of the most recent version of the [Azure SDK for .NET](https://azure.microsoft.com/downloads/).</span></span>

<span data-ttu-id="846f3-125">Skapa ett nytt Windows-konsolprogram i Visual Studio och ger den namnet **GenerateSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="846f3-125">In Visual Studio, create a new Windows console application and name it **GenerateSharedAccessSignatures**.</span></span> <span data-ttu-id="846f3-126">Lägg till referenser till [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) och [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) genom att använda någon av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="846f3-126">Add references to [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) by using one of the following approaches:</span></span>

* <span data-ttu-id="846f3-127">Använd den [NuGet-Pakethanteraren](https://docs.nuget.org/consume/installing-nuget) i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="846f3-127">Use the [NuGet package manager](https://docs.nuget.org/consume/installing-nuget) in Visual Studio.</span></span> <span data-ttu-id="846f3-128">Välj **projekt** > **hantera NuGet-paket**, Sök online efter varje paket (Microsoft.WindowsAzure.ConfigurationManager och WindowsAzure.Storage) och installera dem.</span><span class="sxs-lookup"><span data-stu-id="846f3-128">Select **Project** > **Manage NuGet Packages**, search online for each package (Microsoft.WindowsAzure.ConfigurationManager and WindowsAzure.Storage) and install them.</span></span>
* <span data-ttu-id="846f3-129">Du kan också hitta dessa sammansättningar i installationen av Azure SDK och Lägg till referenser till dem:</span><span class="sxs-lookup"><span data-stu-id="846f3-129">Alternatively, locate these assemblies in your installation of the Azure SDK and add references to them:</span></span>
  * <span data-ttu-id="846f3-130">Microsoft.WindowsAzure.Configuration.dll</span><span class="sxs-lookup"><span data-stu-id="846f3-130">Microsoft.WindowsAzure.Configuration.dll</span></span>
  * <span data-ttu-id="846f3-131">Microsoft.WindowsAzure.Storage.dll</span><span class="sxs-lookup"><span data-stu-id="846f3-131">Microsoft.WindowsAzure.Storage.dll</span></span>

<span data-ttu-id="846f3-132">Lägg till följande längst upp i filen Program.cs **med** direktiven:</span><span class="sxs-lookup"><span data-stu-id="846f3-132">At the top of the Program.cs file, add the following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="846f3-133">Redigera filen app.config så att den innehåller en konfigurationsinställning med en anslutningssträng som pekar på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="846f3-133">Edit the app.config file so that it contains a configuration setting with a connection string that points to your storage account.</span></span> <span data-ttu-id="846f3-134">Filen app.config bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="846f3-134">Your app.config file should look similar to this one:</span></span>

```xml
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
  </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
  </appSettings>
</configuration>
```

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a><span data-ttu-id="846f3-135">Generera en signatur för delad åtkomst URI för en behållare</span><span class="sxs-lookup"><span data-stu-id="846f3-135">Generate a shared access signature URI for a container</span></span>
<span data-ttu-id="846f3-136">Börja med att vi lägga till en metod för att generera en signatur för delad åtkomst på en ny behållare.</span><span class="sxs-lookup"><span data-stu-id="846f3-136">To begin with, we add a method to generate a shared access signature on a new container.</span></span> <span data-ttu-id="846f3-137">I det här fallet är signaturen inte associerad med en lagrad åtkomstprincip så den bedriver URI informationen som anger dess förfallotiden och de behörigheter som beviljas.</span><span class="sxs-lookup"><span data-stu-id="846f3-137">In this case, the signature is not associated with a stored access policy, so it carries on the URI the information indicating its expiry time and the permissions it grants.</span></span>

<span data-ttu-id="846f3-138">Lägg först till kod för att den **Main()** metoden för att autentisera åtkomsten till ditt lagringskonto och skapa en ny behållare:</span><span class="sxs-lookup"><span data-stu-id="846f3-138">First, add code to the **Main()** method to authenticate access to your storage account and create a new container:</span></span>

```csharp
static void Main(string[] args)
{
    //Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create the blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container to use for the sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Insert calls to the methods created below here...

    //Require user input before closing the console window.
    Console.ReadLine();
}
```

<span data-ttu-id="846f3-139">Lägg till en metod som genererar signatur för delad åtkomst för behållaren och returnerar signaturen URI:</span><span class="sxs-lookup"><span data-stu-id="846f3-139">Next, add a method that generates the shared access signature for the container and returns the signature URI:</span></span>

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
    //Set the expiry time and permissions for the container.
    //In this case no start time is specified, so the shared access signature becomes valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Write;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="846f3-140">Lägg till följande rader längst ned i den **Main()** metoden innan anropet till **Console.ReadLine()**att anropa **GetContainerSasUri()** och skriva signaturen URI till konsolfönstret:</span><span class="sxs-lookup"><span data-stu-id="846f3-140">Add the following lines at the bottom of the **Main()** method, before the call to **Console.ReadLine()**, to call **GetContainerSasUri()** and write the signature URI to the console window:</span></span>

```csharp
//Generate a SAS URI for the container, without a stored access policy.
Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="846f3-141">Kompilera och kör om du vill spara signaturen för delad åtkomst URI för den nya behållaren.</span><span class="sxs-lookup"><span data-stu-id="846f3-141">Compile and run to output the shared access signature URI for the new container.</span></span> <span data-ttu-id="846f3-142">URI: N blir liknar följande:</span><span class="sxs-lookup"><span data-stu-id="846f3-142">The URI will be similar to the following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D
```

<span data-ttu-id="846f3-143">När du har kört koden ska signatur för delad åtkomst som du skapade för behållaren gälla för det närmaste dygnet.</span><span class="sxs-lookup"><span data-stu-id="846f3-143">Once you have run the code, the shared access signature you created for the container will be valid for the next 24 hours.</span></span> <span data-ttu-id="846f3-144">Signaturen ger en klient-behörighet för att lista blobbar i behållaren och för att skriva nya blobbar i behållaren.</span><span class="sxs-lookup"><span data-stu-id="846f3-144">The signature grants a client permission to list blobs in the container and to write new blobs to the container.</span></span>

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a><span data-ttu-id="846f3-145">Generera en signatur för delad åtkomst URI för en blob</span><span class="sxs-lookup"><span data-stu-id="846f3-145">Generate a shared access signature URI for a blob</span></span>
<span data-ttu-id="846f3-146">Nu ska skriva vi liknande kod för att skapa en ny blobb i behållaren och generera en signatur för delad åtkomst för den.</span><span class="sxs-lookup"><span data-stu-id="846f3-146">Next, we write similar code to create a new blob within the container and generate a shared access signature for it.</span></span> <span data-ttu-id="846f3-147">Signatur för delad åtkomst är inte associerad med en princip för lagrade åtkomst så att den omfattar starttid, förfallotiden och behörighetsinformation i URI: N.</span><span class="sxs-lookup"><span data-stu-id="846f3-147">This shared access signature is not associated with a stored access policy, so it includes the start time, expiry time, and permission information in the URI.</span></span>

<span data-ttu-id="846f3-148">Lägg till en ny metod som skapar en ny blob och skriver text, och sedan genererar en signatur för delad åtkomst och returnerar signaturen URI:</span><span class="sxs-lookup"><span data-stu-id="846f3-148">Add a new method that creates a new blob and writes some text to it, then generates a shared access signature and returns the signature URI:</span></span>

```csharp
static string GetBlobSasUri(CloudBlobContainer container)
{
    //Get a reference to a blob within the container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

    //Upload text to the blob. If the blob does not yet exist, it will be created.
    //If the blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible to clients via a shared access signature (SAS).";
    blob.UploadText(blobContent);

    //Set the expiry time and permissions for the blob.
    //In this case, the start time is specified as a few minutes in the past, to mitigate clock skew.
    //The shared access signature will be valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

    //Generate the shared access signature on the blob, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="846f3-149">Längst ned i den **Main()** metod, Lägg till följande rader att anropa **GetBlobSasUri()**, innan anropet till **Console.ReadLine()**, och skriva signatur för delad åtkomst URI till konsolfönstret:</span><span class="sxs-lookup"><span data-stu-id="846f3-149">At the bottom of the **Main()** method, add the following lines to call **GetBlobSasUri()**, before the call to **Console.ReadLine()**, and write the shared access signature URI to the console window:</span></span>

```csharp
//Generate a SAS URI for a blob within the container, without a stored access policy.
Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
Console.WriteLine();
```

<span data-ttu-id="846f3-150">Kompilera och kör om du vill spara signaturen för delad åtkomst URI för den nya blobben.</span><span class="sxs-lookup"><span data-stu-id="846f3-150">Compile and run to output the shared access signature URI for the new blob.</span></span> <span data-ttu-id="846f3-151">URI: N blir liknar följande:</span><span class="sxs-lookup"><span data-stu-id="846f3-151">The URI will be similar to the following:</span></span>

```
https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D
```

### <a name="create-a-stored-access-policy-on-the-container"></a><span data-ttu-id="846f3-152">Skapa en princip för lagrade åtkomst till behållaren</span><span class="sxs-lookup"><span data-stu-id="846f3-152">Create a stored access policy on the container</span></span>
<span data-ttu-id="846f3-153">Nu ska vi skapa en princip lagrade åtkomst till behållaren som definierar begränsningarna för alla signaturer för delad åtkomst som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="846f3-153">Now let's create a stored access policy on the container, which will define the constraints for any shared access signatures that are associated with it.</span></span>

<span data-ttu-id="846f3-154">I föregående exempel kan ange vi starttiden (implicit eller explicit) förfallotiden och behörigheterna på signatur för delad åtkomst URI sig själv.</span><span class="sxs-lookup"><span data-stu-id="846f3-154">In the previous examples, we specified the start time (implicitly or explicitly), the expiry time, and the permissions on the shared access signature URI itself.</span></span> <span data-ttu-id="846f3-155">I följande exempel anger vi dessa lagrade åtkomstprincipen, inte signatur för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="846f3-155">In the following examples, we specify these on the stored access policy, not on the shared access signature.</span></span> <span data-ttu-id="846f3-156">På så sätt kan vi kan ändra dessa begränsningar utan att återutfärda signatur för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="846f3-156">Doing so enables us to change these constraints without reissuing the shared access signature.</span></span>

<span data-ttu-id="846f3-157">Det är möjligt att ha en eller flera av begränsningarna för signatur för delad åtkomst och resten på den lagrade åtkomstprincipen.</span><span class="sxs-lookup"><span data-stu-id="846f3-157">It's possible to have one or more of the constraints on the shared access signature, and the remainder on the stored access policy.</span></span> <span data-ttu-id="846f3-158">Men kan du endast ange starttid, förfallotiden och behörigheter i en enda plats eller den andra.</span><span class="sxs-lookup"><span data-stu-id="846f3-158">However, you can only specify the start time, expiry time, and permissions in one place or the other.</span></span> <span data-ttu-id="846f3-159">Du kan inte ange behörigheter på signatur för delad åtkomst och även ange dem på den lagrade åtkomstprincipen.</span><span class="sxs-lookup"><span data-stu-id="846f3-159">For example, you can't specify permissions on the shared access signature and also specify them on the stored access policy.</span></span>

<span data-ttu-id="846f3-160">När du lägger till en princip för lagrade åtkomst till en behållare måste du hämta behållarens befintliga behörigheter, lägga till den nya åtkomstprincipen och sedan ange behållarens behörigheter.</span><span class="sxs-lookup"><span data-stu-id="846f3-160">When you add a stored access policy to a container, you must get the container's existing permissions, add the new access policy, and then set the container's permissions.</span></span>

<span data-ttu-id="846f3-161">Lägg till en ny metod som skapar en ny princip för lagrade åtkomst i en behållare och returnerar namnet på principen:</span><span class="sxs-lookup"><span data-stu-id="846f3-161">Add a new method that creates a new stored access policy on a container and returns the name of the policy:</span></span>

```csharp
static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
    string policyName)
{
    //Get the container's existing permissions.
    BlobContainerPermissions permissions = container.GetPermissions();

    //Create a new shared access policy and define its constraints.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    //Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    container.SetPermissions(permissions);
}
```

<span data-ttu-id="846f3-162">Längst ned i den **Main()** metoden innan anropet till **Console.ReadLine()**, Lägg till följande rader till första Radera alla befintliga principer för åtkomst och sedan anropa den **CreateSharedAccessPolicy()** metod:</span><span class="sxs-lookup"><span data-stu-id="846f3-162">At the bottom of the **Main()** method, before the call to **Console.ReadLine()**, add the following lines to first clear any existing access policies, and then call the **CreateSharedAccessPolicy()** method:</span></span>

```csharp
//Clear any existing access policies on container.
BlobContainerPermissions perms = container.GetPermissions();
perms.SharedAccessPolicies.Clear();
container.SetPermissions(perms);

//Create a new access policy on the container, which may be optionally used to provide constraints for
//shared access signatures on the container and the blob.
string sharedAccessPolicyName = "tutorialpolicy";
CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);
```

<span data-ttu-id="846f3-163">När du avmarkerar åtkomstprinciper i en behållare, måste du först hämta behållarens befintliga behörigheter, och sedan avmarkera behörigheter och sedan ange behörigheten för igen.</span><span class="sxs-lookup"><span data-stu-id="846f3-163">When you clear the access policies on a container, you must first get the container's existing permissions, then clear the permissions, then set the permissions again.</span></span>

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a><span data-ttu-id="846f3-164">Generera en signatur för delad åtkomst URI till behållaren som använder en åtkomstprincip</span><span class="sxs-lookup"><span data-stu-id="846f3-164">Generate a shared access signature URI on the container that uses an access policy</span></span>
<span data-ttu-id="846f3-165">Nu ska skapa vi en annan signatur för delad åtkomst för den behållare som vi skapade tidigare, men den här gången vi associera signaturen med lagrade åtkomstprincipen som vi skapade i föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="846f3-165">Next, we create another shared access signature for the container that we created earlier, but this time we associate the signature with the stored access policy we created in the previous example.</span></span>

<span data-ttu-id="846f3-166">Lägg till en ny metod för att generera en annan signatur för delad åtkomst för behållaren:</span><span class="sxs-lookup"><span data-stu-id="846f3-166">Add a new method to generate another shared access signature for the container:</span></span>

```csharp
static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Generate the shared access signature on the container. In this case, all of the constraints for the
    //shared access signature are specified on the stored access policy.
    string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

    //Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

<span data-ttu-id="846f3-167">Längst ned i den **Main()** metoden innan anropet till **Console.ReadLine()**, Lägg till följande rader att anropa den **GetContainerSasUriWithPolicy** metod:</span><span class="sxs-lookup"><span data-stu-id="846f3-167">At the bottom of the **Main()** method, before the call to **Console.ReadLine()**, add the following lines to call the **GetContainerSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a><span data-ttu-id="846f3-168">Generera en signatur för delad åtkomst URI på Blob som använder en åtkomstprincip</span><span class="sxs-lookup"><span data-stu-id="846f3-168">Generate a Shared Access Signature URI on the Blob That Uses an Access Policy</span></span>
<span data-ttu-id="846f3-169">Slutligen vi lägga till en liknande metod för att skapa en annan blob och generera en signatur för delad åtkomst som är kopplad till en princip för lagrade åtkomst.</span><span class="sxs-lookup"><span data-stu-id="846f3-169">Finally, we add a similar method to create another blob and generate a shared access signature that's associated with a stored access policy.</span></span>

<span data-ttu-id="846f3-170">Lägg till en ny metod för att skapa en blob och generera en signatur för delad åtkomst:</span><span class="sxs-lookup"><span data-stu-id="846f3-170">Add a new method to create a blob and generate a shared access signature:</span></span>

```csharp
static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Get a reference to a blob within the container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

    //Upload text to the blob. If the blob does not yet exist, it will be created.
    //If the blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible to clients via a shared access signature. " +
    "A stored access policy defines the constraints for the signature.";
    MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    ms.Position = 0;
    using (ms)
    {
        blob.UploadFromStream(ms);
    }

    //Generate the shared access signature on the blob.
    string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

<span data-ttu-id="846f3-171">Längst ned i den **Main()** metoden innan anropet till **Console.ReadLine()**, Lägg till följande rader att anropa den **GetBlobSasUriWithPolicy** metod:</span><span class="sxs-lookup"><span data-stu-id="846f3-171">At the bottom of the **Main()** method, before the call to **Console.ReadLine()**, add the following lines to call the **GetBlobSasUriWithPolicy** method:</span></span>

```csharp
//Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

<span data-ttu-id="846f3-172">Den **Main()** metod bör nu se ut så här i sin helhet.</span><span class="sxs-lookup"><span data-stu-id="846f3-172">The **Main()** method should now look like this in its entirety.</span></span> <span data-ttu-id="846f3-173">Kör det skriva signatur för delad åtkomst URI: er till konsolfönstret, och sedan kopiera och klistra in dem i en textfil för användning i den andra delen av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="846f3-173">Run it to write the shared access signature URIs to the console window, then copy and paste them into a text file for use in the second part of this tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    //Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create the blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container to use for the sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    Console.ReadLine();
}
```

<span data-ttu-id="846f3-174">När du kör konsolprogram GenerateSharedAccessSignatures visas utdata som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="846f3-174">When you run the GenerateSharedAccessSignatures console application, you'll see output similar to the following.</span></span> <span data-ttu-id="846f3-175">Dessa är signaturer för delad åtkomst som du använder i en del 2 av kursen.</span><span class="sxs-lookup"><span data-stu-id="846f3-175">These are the shared access signatures you use in Part 2 of the tutorial.</span></span>

```
Container SAS URI: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=pFlEZD%2F6sJTNLxD%2FQ26Hh85j%2FzYPxZav6mP1KJwnvJE%3D&se=2017-05-16T16%3A16%3A47Z&sp=wl

Blob SAS URI: https://storagesample.blob.core.windows.net/sascontainer/sasblob.txt?sv=2016-05-31&sr=b&sig=%2FiBWAZbXESzCMvRcm7JwJBK0gT0BtPSWEq4pRwmlBRI%3D&st=2017-05-15T16%3A11%3A48Z&se=2017-05-16T16%3A16%3A48Z&sp=rw

Container SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&si=tutorialpolicy&sig=aMb6rKDvvpfiGVsZI2rCmyUra6ZPpq%2BZ%2FLyTgAeec%2Bk%3D

Blob SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer/sasblobpolicy.txt?sv=2016-05-31&sr=b&si=tutorialpolicy&sig=%2FkTWkT23SS45%2FoF4bK2mqXkN%2BPKs%2FyHuzkfQ4GFoZVU%3D
```

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a><span data-ttu-id="846f3-176">Del 2: Skapa ett konsolprogram att testa signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="846f3-176">Part 2: Create a console application to test the shared access signatures</span></span>
<span data-ttu-id="846f3-177">Om du vill testa signaturer för delad åtkomst som skapats i föregående exempel skapar vi ett andra konsolprogram som använder signaturer för att utföra åtgärder på behållaren och en blob.</span><span class="sxs-lookup"><span data-stu-id="846f3-177">To test the shared access signatures created in the previous examples, we create a second console application that uses the signatures to perform operations on the container and on a blob.</span></span>

> [!NOTE]
> <span data-ttu-id="846f3-178">Om mer än 24 timmar har gått sedan du slutfört den första delen av kursen, kommer signaturer som du genererade inte längre giltig.</span><span class="sxs-lookup"><span data-stu-id="846f3-178">If more than 24 hours have passed since you completed the first part of the tutorial, the signatures you generated will no longer be valid.</span></span> <span data-ttu-id="846f3-179">I det här fallet bör du köra koden i det första konsolprogrammet att generera en ny delad åtkomstsignaturer för användning i den andra delen av kursen.</span><span class="sxs-lookup"><span data-stu-id="846f3-179">In this case, you should run the code in the first console application to generate fresh shared access signatures for use in the second part of the tutorial.</span></span>
>

<span data-ttu-id="846f3-180">Skapa ett nytt Windows-konsolprogram i Visual Studio och ger den namnet **ConsumeSharedAccessSignatures**.</span><span class="sxs-lookup"><span data-stu-id="846f3-180">In Visual Studio, create a new Windows console application and name it **ConsumeSharedAccessSignatures**.</span></span> <span data-ttu-id="846f3-181">Lägg till referenser till [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) och [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), som du gjorde tidigare.</span><span class="sxs-lookup"><span data-stu-id="846f3-181">Add references to [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) and [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), as you did previously.</span></span>

<span data-ttu-id="846f3-182">Lägg till följande längst upp i filen Program.cs **med** direktiven:</span><span class="sxs-lookup"><span data-stu-id="846f3-182">At the top of the Program.cs file, add the following **using** directives:</span></span>

```csharp
using System.IO;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="846f3-183">I brödtexten för den **Main()** metod, Lägg till följande strängkonstanter, ändra deras värden till signaturer för delad åtkomst som du genererade i del 1 av kursen.</span><span class="sxs-lookup"><span data-stu-id="846f3-183">In the body of the **Main()** method, add the following string constants, changing their values to the shared access signatures you generated in part 1 of the tutorial.</span></span>

```csharp
static void Main(string[] args)
{
    const string containerSAS = "<your container SAS>";
    const string blobSAS = "<your blob SAS>";
    const string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    const string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
}
```

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a><span data-ttu-id="846f3-184">Lägga till en metod om du vill försöka behållaren åtgärder med hjälp av en signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="846f3-184">Add a method to try container operations using a shared access signature</span></span>
<span data-ttu-id="846f3-185">Nu ska vi lägga till en metod som testar vissa åtgärder för behållaren med hjälp av en signatur för delad åtkomst för behållaren.</span><span class="sxs-lookup"><span data-stu-id="846f3-185">Next, we add a method that tests some container operations using a shared access signature for the container.</span></span> <span data-ttu-id="846f3-186">Signatur för delad åtkomst används för att returnera en referens till behållaren måste autentisera åtkomst till behållaren baserat på enbart signaturen.</span><span class="sxs-lookup"><span data-stu-id="846f3-186">The shared access signature is used to return a reference to the container, authenticating access to the container based on the signature alone.</span></span>

<span data-ttu-id="846f3-187">Lägg till följande metod i Program.cs:</span><span class="sxs-lookup"><span data-stu-id="846f3-187">Add the following method to Program.cs:</span></span>

```csharp
static void UseContainerSAS(string sas)
{
    //Try performing container operations with the SAS provided.

    //Return a reference to the container using the SAS URI.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

    //Create a list to store blob URIs returned by a listing operation on the container.
    List<ICloudBlob> blobList = new List<ICloudBlob>();

    //Write operation: write a new blob to the container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
        string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
        blob.UploadText(blobContent);

        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //List operation: List the blobs in the container.
    try
    {
        foreach (ICloudBlob blob in container.ListBlobs())
        {
            blobList.Add(blob);
        }
        Console.WriteLine("List operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("List operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Get a reference to one of the blobs in the container and read it.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        MemoryStream msRead = new MemoryStream();
        msRead.Position = 0;
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            Console.WriteLine(msRead.Length);
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    Console.WriteLine();

    //Delete operation: Delete a blob in the container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="846f3-188">Uppdatering av **Main()** metod för att anropa **UseContainerSAS()** med både signaturer för delad åtkomst som du skapade på behållaren:</span><span class="sxs-lookup"><span data-stu-id="846f3-188">Update the **Main()** method to call **UseContainerSAS()** with both of the shared access signatures you created on the container:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call the test methods with the shared access signatures created on the container, with and without the access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    Console.ReadLine();
}
```

### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a><span data-ttu-id="846f3-189">Lägga till en metod om du vill försöka blob-åtgärder med hjälp av en signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="846f3-189">Add a method to try blob operations using a shared access signature</span></span>
<span data-ttu-id="846f3-190">Slutligen vi lägga till en metod som testar vissa blob-åtgärder med hjälp av en signatur för delad åtkomst på blob.</span><span class="sxs-lookup"><span data-stu-id="846f3-190">Finally, we add a method that tests some blob operations using a shared access signature on the blob.</span></span> <span data-ttu-id="846f3-191">I detta fall kan vi använda konstruktorn **CloudBlockBlob(String)**, och skickar signatur för delad åtkomst att returnera en referens till blob.</span><span class="sxs-lookup"><span data-stu-id="846f3-191">In this case, we use the constructor **CloudBlockBlob(String)**, passing in the shared access signature, to return a reference to the blob.</span></span> <span data-ttu-id="846f3-192">Inga andra autentisering krävs. den är baserad på enbart signaturen.</span><span class="sxs-lookup"><span data-stu-id="846f3-192">No other authentication is required; it's based on the signature alone.</span></span>

<span data-ttu-id="846f3-193">Lägg till följande metod i Program.cs:</span><span class="sxs-lookup"><span data-stu-id="846f3-193">Add the following method to Program.cs:</span></span>

```csharp
static void UseBlobSAS(string sas)
{
    //Try performing blob operations using the SAS provided.

    //Return a reference to the blob using the SAS URI.
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

    //Write operation: Write a new blob to the container.
    try
    {
        string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
        MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        msWrite.Position = 0;
        using (msWrite)
        {
            blob.UploadFromStream(msWrite);
        }
        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Read the contents of the blob.
    try
    {
        MemoryStream msRead = new MemoryStream();
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            msRead.Position = 0;
            using (StreamReader reader = new StreamReader(msRead, true))
            {
                string line;
                while ((line = reader.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Delete operation: Delete the blob.
    try
    {
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="846f3-194">Uppdatering av **Main()** metod för att anropa **UseBlobSAS()** med både signaturer för delad åtkomst som du skapade på blob:</span><span class="sxs-lookup"><span data-stu-id="846f3-194">Update the **Main()** method to call **UseBlobSAS()** with both of the shared access signatures that you created on the blob:</span></span>

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call the test methods with the shared access signatures created on the container, with and without the access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
    UseBlobSAS(blobSAS);
    UseBlobSAS(blobSASWithAccessPolicy);

    Console.ReadLine();
}
```

<span data-ttu-id="846f3-195">Köra konsolprogrammet och se utdata att se vilka åtgärder som är tillåtna för vilka signaturer.</span><span class="sxs-lookup"><span data-stu-id="846f3-195">Run the console application and observe the output to see which operations are permitted for which signatures.</span></span> <span data-ttu-id="846f3-196">Utdata i konsolfönstret ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="846f3-196">The output in the console window will look similar to the following:</span></span>

```
Write operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

List operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

Read operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: The remote server returned an error: (403) Forbidden.

Delete operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: The remote server returned an error: (403) Forbidden.

...
```

## <a name="next-steps"></a><span data-ttu-id="846f3-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="846f3-197">Next Steps</span></span>

* [<span data-ttu-id="846f3-198">Signaturer för delad åtkomst, del 1: Förstå SAS-modellen</span><span class="sxs-lookup"><span data-stu-id="846f3-198">Shared Access Signatures, Part 1: Understanding the SAS Model</span></span>](storage-dotnet-shared-access-signature-part-1.md)
* [<span data-ttu-id="846f3-199">Hantera anonym läsbehörighet till behållare och blobbar</span><span class="sxs-lookup"><span data-stu-id="846f3-199">Manage anonymous read access to containers and blobs</span></span>](storage-manage-access-to-resources.md)
* [<span data-ttu-id="846f3-200">Delegera åtkomst med en signatur för delad åtkomst (REST-API)</span><span class="sxs-lookup"><span data-stu-id="846f3-200">Delegating access with a shared access signature (REST API)</span></span>](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [<span data-ttu-id="846f3-201">Introduktion till tabellen och kön SAS</span><span class="sxs-lookup"><span data-stu-id="846f3-201">Introducing Table and Queue SAS</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
